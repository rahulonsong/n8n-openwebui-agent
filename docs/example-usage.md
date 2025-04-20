# Example Usage

This document demonstrates how to use the n8n OpenWebUI agent integration in practice.

## Basic Usage

Once you've set up both n8n and OpenWebUI with the integration, you can start using the agent in your chats:

1. Open OpenWebUI at http://localhost:3000
2. Start a new chat
3. Type your message as you normally would
4. Access the n8n agent function by either:
   - Clicking on the "Functions" icon in the message input area
   - Using a keyboard shortcut (if configured)

## Example Conversations

### General Knowledge Query

**User**: `/n8n_pipe Can you explain quantum computing in simple terms?`

**Agent**: _The agent processes this through n8n and returns a response like:_

```
Quantum computing is like having a super-powerful calculator that works differently than regular computers.

While normal computers use bits (0s and 1s) to process information one step at a time, quantum computers use "qubits" that can be both 0 and 1 at the same time (called superposition).

This means quantum computers can look at many possibilities simultaneously, making them potentially much faster at solving certain complex problems like:
- Breaking encryption
- Simulating molecules for drug discovery
- Optimizing complicated systems like traffic flow

However, quantum computers are still being developed and aren't yet practical for everyday use. They require special conditions like extreme cold to work properly.
```

### Using Web Search Tool

**User**: `/n8n_pipe What were the major technology announcements this week?`

**Agent**: _The agent uses the web search tool to find current information:_

```
Based on my web search, here are the major technology announcements this week:

1. Apple announced its new AI features called "Apple Intelligence" coming to iOS 18, including improved Siri capabilities and system-wide AI integration
2. Microsoft released a new version of Windows 11 with enhanced security features
3. Google unveiled updates to its search algorithm focusing on more accurate local results
4. NVIDIA announced new GPU models targeted at AI development
5. Meta (Facebook) introduced advanced privacy controls for its platforms

These announcements represent significant advancements in AI integration, security, and user privacy across major technology platforms.
```

### Maintaining Context in Conversations

**User**: `/n8n_pipe I'm planning to build a simple website for my small business. What technologies would you recommend?`

**Agent**: _Response about web technologies, frameworks, hosting options, etc._

**User**: `/n8n_pipe What if I want to add e-commerce capabilities later?`

**Agent**: _The agent remembers the previous context about a small business website and recommends e-commerce solutions accordingly:_

```
For adding e-commerce capabilities to your small business website later, you have several good options:

1. If you build with WordPress: Add WooCommerce plugin (free, highly customizable)
2. If using a website builder: Most (like Wix, Squarespace) offer e-commerce upgrades
3. For custom sites: Consider integrating Shopify Lite or PayPal Commerce

Since you're starting with a simple website, I'd recommend:
- Planning your database structure to accommodate products from the beginning
- Using a CMS that has good e-commerce extension options
- Selecting hosting that can scale with increased traffic

This approach gives you a simple start while making future expansion easier without rebuilding your entire site.
```

## Using Advanced Features

### Document Q&A

To use the RAG (Retrieval Augmented Generation) feature with documents:

1. First, upload documents to OpenWebUI's document library
2. Then ask the agent about these documents:

**User**: `/n8n_pipe #my_business_plan.pdf What is the projected revenue for Q2?`

**Agent**: _Searches through the specified document and provides the answer based on its contents_

### Technical Troubleshooting

**User**: `/n8n_pipe I'm getting a "500 Internal Server Error" when trying to access the function. What should I check?`

**Agent**: _Provides troubleshooting steps specific to the n8n integration:_

```
If you're getting a "500 Internal Server Error" when using the n8n function, here are specific things to check:

1. Verify n8n is running and accessible (try opening http://localhost:5678)
2. Check that your webhook is activated in n8n (look for the green "active" status)
3. Examine n8n logs for any error messages
4. Ensure the webhook URL in OpenWebUI settings is correct and includes the complete path
5. Verify authentication credentials match between OpenWebUI and n8n
6. Check that your PostgreSQL database is connected properly
7. Restart both n8n and OpenWebUI services

Most commonly, this error occurs due to authentication issues or the n8n workflow not being properly activated.
```

## Tips for Best Performance

1. **Start with simple prompts** to test that the integration is working
2. **Use specific questions** for more accurate results
3. **Leverage session memory** by continuing the conversation naturally
4. **Restart both services** if you encounter persistent issues
5. **Check your n8n logs** for detailed error information
6. **Update the workflow** as needed to customize the agent's capabilities
