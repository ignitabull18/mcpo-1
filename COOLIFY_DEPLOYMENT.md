# MCPO Coolify Deployment Guide

This repository contains a comprehensive MCPO (Model Context Protocol OpenAPI Proxy) setup with multiple MCP servers, ready for deployment on Coolify.

## 🚀 Quick Deploy to Coolify

### Repository Information
- **Repository**: `https://github.com/ignitabull18/mcpo-1.git`
- **Branch**: `main`
- **Build Pack**: Docker (auto-detected)

### 1. Create Application in Coolify

1. Go to your Coolify dashboard: `https://coolify1.ignitabull.org`
2. Click **New Resource** → **Application**
3. Choose **Public Repository**
4. Enter repository URL: `https://github.com/ignitabull18/mcpo-1.git`
5. Branch: `main`
6. Build Pack: Docker (will auto-detect)

### 2. Configure Domain
- **Domain**: `mcpo.ignitabull.org`
- **Port**: `8000`
- **HTTPS**: ✅ Enable SSL/TLS

### 3. Environment Variables

Set these variables in Coolify's Environment Variables section:

#### Required Variables
```bash
MCPO_API_KEY=your-secure-mcpo-api-key-here
```

#### Coolify Integration (Pre-configured)
The Coolify MCP server is already configured with your credentials:
- COOLIFY_BASE_URL: `https://coolify1.ignitabull.org`
- COOLIFY_TOKEN: `4|Jz2gHZhbWFPubozPVGeJiULNawsfDU1VvtYazBnffc1cbbf3`

#### Optional Variables (Enable specific MCP servers)
Only set the variables for services you want to use:

```bash
# GitHub Integration
GITHUB_PERSONAL_ACCESS_TOKEN=your-github-token

# AI Model APIs
OPENAI_API_KEY=your-openai-api-key
ANTHROPIC_API_KEY=your-anthropic-api-key
GOOGLE_API_KEY=your-google-api-key
MISTRAL_API_KEY=your-mistral-api-key
PERPLEXITY_API_KEY=your-perplexity-api-key
OPENROUTER_API_KEY=your-openrouter-api-key
XAI_API_KEY=your-xai-api-key
AZURE_OPENAI_API_KEY=your-azure-openai-api-key

# Supabase
SUPABASE_ACCESS_TOKEN=your-supabase-access-token

# N8N Integration
N8N_API_URL=your-n8n-api-url
N8N_API_KEY=your-n8n-api-key
N8N_WEBHOOK_USERNAME=your-n8n-webhook-username
N8N_WEBHOOK_PASSWORD=your-n8n-webhook-password

# Smithery Services
SMITHERY_API_KEY=your-smithery-api-key
SMITHERY_PROFILE=your-smithery-profile
SMITHERY_JINA_PROFILE=your-smithery-jina-profile

# Search APIs
BRAVE_API_KEY=your-brave-search-api-key

# Communication
SLACK_BOT_TOKEN=your-slack-bot-token
```

## 📋 Available MCP Servers

### Always Available (No API Key Required)
- **fetch**: Web content fetching
- **time**: Time and timezone utilities
- **filesystem**: File system operations
- **memory**: In-memory storage
- **sequential-thinking**: Sequential reasoning
- **browser-tools-mcp**: Browser automation tools
- **context7**: Context management
- **deepwiki**: DeepWiki integration

### SSE-Based Servers
- **openmemory**: `https://openmemory-api.ignitabull.org/mcp/cursor/sse/root`
- **mindsdb**: `http://mindsdb.ignitabull.org:47337/sse`

### API Key Required
- **coolify**: Coolify management (pre-configured)
- **github**: GitHub integration
- **supabase**: Supabase database operations
- **taskmaster-ai**: AI task management
- **kit-mcp**: OpenAI-powered kit
- **brave-search**: Brave search engine
- **slack**: Slack integration
- **n8n**: N8N workflow automation
- **toolbox**: Smithery toolbox
- **firecrawl-mcp-server**: Web scraping
- **jina-ai-mcp-server**: Jina AI services

## 🔧 Deployment Process

### Step 1: Deploy
1. Click **Deploy** in Coolify
2. Wait for the build to complete
3. The application will be available at `https://mcpo.ignitabull.org`

### Step 2: Verify Installation
- **Main API Docs**: `https://mcpo.ignitabull.org/docs`
- **Health Check**: `https://mcpo.ignitabull.org/` (should return health status)

### Step 3: Test MCP Servers
Individual server documentation will be available at:
- `https://mcpo.ignitabull.org/{server-name}/docs`

Examples:
- `https://mcpo.ignitabull.org/coolify/docs` - Coolify management
- `https://mcpo.ignitabull.org/fetch/docs` - Web fetching
- `https://mcpo.ignitabull.org/github/docs` - GitHub integration (if API key set)
- `https://mcpo.ignitabull.org/time/docs` - Time utilities

## 🔍 Troubleshooting

### Common Issues

1. **Server Not Starting**
   - Check if `MCPO_API_KEY` is set
   - Verify environment variables in Coolify

2. **MCP Server Missing**
   - Check if required environment variables are set
   - Look at application logs in Coolify

3. **API Errors**
   - Verify API keys are correct
   - Check if the external service is accessible

### Checking Logs
In Coolify:
1. Go to your application
2. Click on **Logs** tab
3. Look for any error messages during startup

### Enabling/Disabling Servers
- Add environment variables to enable new servers
- Remove environment variables to disable servers
- Restart the application in Coolify after changes

## 🔐 Security Notes

1. **API Keys**: Always use encrypted environment variables in Coolify
2. **MCPO_API_KEY**: Use a strong, unique API key
3. **External APIs**: Regularly rotate API keys
4. **Access Control**: The Coolify integration has admin access to your Coolify instance

## 🔄 Updates

To update the configuration:
1. Modify `config/mcp-config.json` in the repository
2. Commit and push changes
3. Coolify will automatically rebuild and deploy

## 📊 Monitoring

- **Health Endpoint**: `https://mcpo.ignitabull.org/`
- **API Documentation**: `https://mcpo.ignitabull.org/docs`
- **Individual Server Status**: Check each `/docs` endpoint

## 🤝 Integration with OpenWebUI

After deployment, you can integrate with OpenWebUI by configuring it to use:
- **MCPO Base URL**: `https://mcpo.ignitabull.org`
- **API Key**: Your `MCPO_API_KEY`

This will provide your OpenWebUI instance with access to all configured MCP servers through a unified OpenAPI interface.
