# MCPO Deployment for Coolify

This repository is configured for easy deployment on Coolify with comprehensive MCP server support.

## Quick Deploy to Coolify

### 1. Repository Setup
- Repository URL: `https://github.com/ignitabull18/mcpo-1.git`
- Branch: `main`
- Build Pack: Docker (uses existing Dockerfile)

### 2. Environment Variables (Set in Coolify UI)

#### Required:
```
MCPO_API_KEY=your-secure-api-key-here
```

#### Optional (only set if you plan to use these services):
```
BRAVE_API_KEY=your-brave-search-api-key
GITHUB_PERSONAL_ACCESS_TOKEN=your-github-token
SLACK_BOT_TOKEN=your-slack-bot-token
OPENAI_API_KEY=your-openai-api-key
ANTHROPIC_API_KEY=your-anthropic-api-key
TAVILY_API_KEY=your-tavily-search-api-key
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
```

### 3. Domain Configuration
- Domain: `mcpo.ignitabull.org`
- Enable HTTPS/SSL
- Port: 8000

### 4. Deployment Command
The container will automatically start with:
```bash
mcpo --host 0.0.0.0 --port 8000 --config /app/config/mcp-config.json --api-key $MCPO_API_KEY
```

## Environment Variable Management in Coolify

✅ **All environment variables are managed through Coolify's UI:**
- Go to your application → **Environment Variables** tab
- Add variables one by one using the form
- Mark sensitive variables (API keys) as "Encrypted"
- Variables are automatically injected into the container
- No need to manually edit files or upload .env files

## Available MCP Servers

### Always Available (no API key required):
- **fetch** - Web scraping and content fetching
- **time** - Time and date utilities  
- **filesystem** - File system operations
- **memory** - Persistent memory for conversations
- **sequential-thinking** - Step-by-step reasoning

### API Key Required (enable by setting environment variables):
- **brave-search** - Web search via Brave API
- **github** - GitHub repository operations
- **slack** - Slack integration
- **openai** - OpenAI model integration
- **anthropic** - Claude model integration

## Testing Your Deployment

After deployment, verify:
1. Main API docs: `https://mcpo.ignitabull.org/docs`
2. Individual server docs: `https://mcpo.ignitabull.org/{server-name}/docs`
3. Health check: `https://mcpo.ignitabull.org/health`

## Adding More Servers

To add additional MCP servers:
1. Edit `config/mcp-config.json` in this repository
2. Add any required environment variables in Coolify UI
3. Redeploy the application

## Security Notes

- Always use encrypted environment variables for API keys in Coolify
- The MCPO_API_KEY is required for accessing the API endpoints
- Each MCP server that requires API keys will only be available if the corresponding environment variable is set
