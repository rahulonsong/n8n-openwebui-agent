# Setup Guide for n8n OpenWebUI Integration

This guide provides detailed instructions for setting up the n8n OpenWebUI integration.

## Setting Up n8n

### Installation

If you don't already have n8n installed, you can set it up using Docker:

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

Or install it using npm:

```bash
npm install n8n -g
n8n start
```

Your n8n instance will be available at http://localhost:5678

## Setting Up OpenWebUI

### Installation

Install OpenWebUI using Docker:

```bash
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

Your OpenWebUI instance will be available at http://localhost:3000

## Configuring the Integration

### 1. Import the Workflow Template

1. Download the `Open_WebUI_Agent_Template.json` from this repository
2. Go to your n8n instance (http://localhost:5678)
3. Click on "Workflows" in the left sidebar
4. Click "Import from File" and select the downloaded template

### 2. Configure the Workflow

#### Webhook Configuration

1. Click on the "Webhook" node in the workflow
2. Configure the webhook path (default is `invoke-n8n-agent`)
3. Set up header authentication:
   - Authentication type: Header Auth
   - Header name: Authorization
   - Header value: Bearer YOUR_SECRET_TOKEN
4. Save the node settings

#### Language Model Configuration

1. Click on the "OpenAI Chat Model" node
2. Add your OpenAI API credentials or configure connection to a self-hosted model
3. Select the model you want to use (e.g., gpt-4, gpt-3.5-turbo)
4. Save the node settings

#### PostgreSQL Memory Configuration

1. Click on the "Postgres Chat Memory" node
2. Configure the PostgreSQL connection:
   - Host: your_postgres_host
   - Database: your_database
   - User: your_user
   - Password: your_password
3. Ensure the session key is set to `={{ $json.body.sessionId }}`
4. Save the node settings

### 3. Activate the Workflow

1. Click the "Activate" button in the top right corner of the workflow editor
2. The webhook is now active and ready to receive requests

### 4. Set Up the OpenWebUI Function

1. Go to your OpenWebUI instance (http://localhost:3000)
2. Click on your profile icon in the bottom left corner
3. Go to "Admin Panel" -> "Functions"
4. Import the n8n Pipe function:
   - Click the "+" button
   - Select "Import from JSON"
   - Copy and paste the contents of `n8n-pipe-function.json` from this repository
   - Click "Import"
5. Click on the settings icon for the n8n Pipe function
6. Configure the function:
   - n8n webhook URL: `http://your-n8n-host:5678/webhook/invoke-n8n-agent`
   - Bearer token: `YOUR_SECRET_TOKEN` (same as in n8n webhook)
   - Input field key: `chatInput`
   - Output field key: `output`
7. Save the settings

## Testing the Integration

1. Open a chat in OpenWebUI
2. Type a message
3. Use the n8n agent function in your conversation
4. The n8n agent should respond based on your configuration

## Troubleshooting

### Webhook Connection Issues

If OpenWebUI cannot connect to your n8n webhook:

1. Ensure your n8n instance is accessible from the machine running OpenWebUI
2. Check if the webhook URL is correct, including the protocol (http/https)
3. Verify the webhook path matches what's configured in n8n

### Authentication Errors

If you get authentication errors:

1. Make sure the bearer token is exactly the same in both n8n and OpenWebUI
2. Check that the Authorization header is properly formatted as `Bearer YOUR_SECRET_TOKEN`

### Database Connectivity

If there are issues with PostgreSQL memory:

1. Verify your PostgreSQL connection parameters
2. Check if the necessary tables are created (n8n should create them automatically)
3. Ensure your PostgreSQL server allows connections from n8n

### Using Local LLMs

If you want to use local LLMs instead of OpenAI:

1. Set up Ollama or another local LLM provider
2. Configure an API compatible with the OpenAI format (many local providers offer this)
3. Replace the OpenAI credentials with the local API endpoint
4. Test with a simple model before scaling to more complex ones
