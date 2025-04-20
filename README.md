# n8n OpenWebUI Agent Integration

This repository contains an n8n workflow template that allows you to integrate n8n agents with [Open WebUI](https://openwebui.com), enabling you to chat with your n8n-powered AI agents directly through the Open WebUI interface.

If you self-host n8n and have local LLMs through something like Ollama, this solution can be used to run n8n powered agents entirely offline!

## Overview

The `Open_WebUI_Agent_Template.json` workflow provides a ready-to-use template for creating an n8n agent that can be accessed through Open WebUI's function calling system. This integration enables:

- Exposing your n8n AI agents as a service through a webhook
- Maintaining conversation history with Postgres memory
- Utilizing language models for responses (OpenAI or self-hosted models)
- Including tools like web search capabilities
- Seamless integration with the Open WebUI chat interface

## Prerequisites

Before setting up this integration, make sure you have:

- An n8n instance (self-hosted or cloud)
- Open WebUI set up (typically on http://localhost:3000)
- API credentials for your chosen language model (OpenAI or alternative)
- PostgreSQL database for conversation memory (optional but recommended)

## Quickstart

1. **Install Open WebUI** (if not already installed):

   - Follow the [Open WebUI installation instructions](https://github.com/open-webui/open-webui#how-to-install-)
   - Typically, you can use Docker:
     ```bash
     docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
     ```

2. **Import the n8n Workflow Template**:

   - Download the `Open_WebUI_Agent_Template.json` from this repository
   - In your n8n instance, go to "Workflows"
   - Click "Import from File" and select the downloaded template file

3. **Configure the n8n Workflow**:

   - Set up your AI model credentials (OpenAI or alternative)
   - Configure PostgreSQL database connection for conversation memory
   - Set up header authentication for the webhook
   - Activate the workflow to make the webhook endpoint available

4. **Set Up the Open WebUI Function**:

   - In Open WebUI, go to "Profile (bottom left) -> Admin Panel -> Functions"
   - Import the n8n Pipe function
   - Click on the settings icon for the n8n Pipe function
   - Enter your n8n webhook URL, bearer token, input field key, and output field key

5. **Test the Integration**:
   - Open a chat in Open WebUI
   - Use the n8n agent function in your conversation
   - The n8n agent should respond based on your configuration

## Documentation

For more detailed information, check out these documentation files:

- [Detailed Setup Guide](docs/setup-guide.md) - Step-by-step instructions for setting up the integration
- [Integration Workflow Diagram](docs/integration-diagram.md) - Visual representation of how the components work together
- [Example Usage](docs/example-usage.md) - Examples of how to use the integration in practice

## Detailed Setup Instructions

### Setting Up n8n Workflow

1. **Configure the Webhook Node**:

   - Set the webhook path (default is `invoke-n8n-agent`)
   - Configure header authentication with a secure bearer token
   - Note the full webhook URL for later use with Open WebUI

2. **Configure Language Model**:

   - Add your OpenAI API credentials or configure connection to a self-hosted model
   - Adjust model parameters as needed

3. **Set Up PostgreSQL Memory** (optional but recommended):

   - Configure the PostgreSQL connection
   - Ensure the session key is set to `={{ $json.body.sessionId }}`

4. **Customize Agent Tools**:

   - The template includes a web search tool which you can customize
   - Add additional tools as needed for your specific use case

5. **Activate the Workflow**:
   - Save and activate the workflow to make the webhook accessible

### Setting Up Open WebUI Function

1. **Import the n8n Pipe Function**:

   - Go to Open WebUI Admin Panel -> Functions
   - Use the import function to add the n8n Pipe

2. **Configure Function Settings**:
   - Set the n8n webhook URL (e.g., `http://localhost:5678/webhook/invoke-n8n-agent`)
   - Enter the same bearer token used in the n8n webhook configuration
   - Set input field key to match your n8n workflow (typically `chatInput`)
   - Set output field key to match your n8n workflow (typically `output`)

## How It Works

1. When a user interacts with the n8n function in Open WebUI, it sends a request to the n8n webhook.
2. The n8n workflow processes the request, using the configured AI model and tools.
3. The workflow maintains conversation context using PostgreSQL memory.
4. The response is sent back to Open WebUI and displayed to the user.

## Troubleshooting

If you encounter issues with the integration:

1. **Check Webhook Connectivity**:

   - Ensure your n8n instance is accessible from Open WebUI
   - Verify the webhook URL in the Open WebUI function settings

2. **Authentication Issues**:

   - Confirm the bearer token matches between n8n and Open WebUI

3. **Database Connectivity**:

   - Check PostgreSQL connection parameters
   - Ensure your database has the necessary tables for chat memory

4. **Response Format**:
   - Verify the output field key matches in both n8n and Open WebUI

## License

This project is licensed under the MIT License - see the LICENSE file for details.
