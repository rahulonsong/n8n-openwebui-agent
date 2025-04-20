# Integration Workflow Diagram

The following diagram illustrates the integration flow between OpenWebUI and n8n:

```mermaid
sequenceDiagram
    actor User
    participant OpenWebUI
    participant n8nWebhook as "n8n Webhook"
    participant Agent as "n8n AI Agent"
    participant DB as "PostgreSQL Memory"
    participant LLM as "Language Model"
    participant Tools as "External Tools"

    User->>OpenWebUI: Sends a message
    OpenWebUI->>n8nWebhook: HTTP POST with message and sessionId
    Note over OpenWebUI,n8nWebhook: Authentication via Bearer Token

    n8nWebhook->>Agent: Forwards request to agent
    Agent->>DB: Retrieves conversation history
    Agent->>LLM: Sends prompt with context

    opt If needed
        LLM->>Tools: Calls external tools (e.g., web search)
        Tools->>LLM: Returns tool results
    end

    LLM->>Agent: Returns response
    Agent->>DB: Saves new message to history
    Agent->>n8nWebhook: Returns formatted response
    n8nWebhook->>OpenWebUI: Returns JSON with output field
    OpenWebUI->>User: Displays response in chat
```

## System Architecture

```mermaid
flowchart TD
    subgraph "User Interface"
        A[OpenWebUI]
    end

    subgraph "n8n Platform"
        B[Webhook Endpoint]
        C[Memory Management]
        D[AI Agent]
        E[Tool Integration]
    end

    subgraph "External Services"
        F[Language Model]
        G[PostgreSQL Database]
        H[Search API]
        I[Other Tools]
    end

    A <-->|HTTP Requests| B
    B --> D
    D <--> C
    D <--> E

    C <--> G
    D <--> F
    E <--> H
    E <--> I
```

## Function Setup Flow

```mermaid
flowchart LR
    A[Import n8n Workflow] --> B[Configure Webhook]
    B --> C[Set Up LLM Credentials]
    C --> D[Configure DB Connection]
    D --> E[Activate Workflow]

    F[Import Function to OpenWebUI] --> G[Configure Valves]
    G --> H[Set Webhook URL]
    H --> I[Set Bearer Token]
    I --> J[Configure Field Keys]

    E --- F
```

These diagrams help visualize how the different components of the integration work together and the setup process.
