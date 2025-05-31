# MCPO Coolify Deployment Guide

## Overview
This guide provides comprehensive instructions for deploying MCPO (Multi-Context Protocol Orchestrator) on Coolify with full MCP server integration and OpenAPI support.

## Prerequisites
- Coolify instance running and accessible
- GitHub repository access
- Node.js and Python environment support
- Domain configured (mcpo.ignitabull.org)

## Quick Start Deployment

### 1. Coolify Application Setup
1. **Create New Application**: 
   - Go to Coolify dashboard → Projects → Create Application
   - Select GitHub source: `ignitabull18/mcpo-1`
   - Branch: `main`
   - Set domain: `https://mcpo.ignitabull.org`

2. **Build Configuration**:
   - Build pack: `nixpacks` (auto-detected)
   - Port: `8000`
   - Health check: `/docs` endpoint

### 2. Environment Variables
Set these in Coolify environment configuration:
```bash
# Basic Configuration
NODE_VERSION=22
NODE_OPTIONS=--max-old-space-size=4096
PORT=8000

# MCP Configuration (optional - can use config file instead)
MCPO_CONFIG_PATH=/app/config/mcpo-config.json
MCPO_API_KEY=your-secure-api-key
MCPO_CORS_ORIGINS=*

# External Service Tokens (for MCP servers)
GITHUB_PERSONAL_ACCESS_TOKEN=your_github_token
SLACK_BOT_TOKEN=your_slack_token
SLACK_TEAM_ID=your_slack_team_id
POSTGRES_CONNECTION_STRING=your_postgres_connection
BRAVE_API_KEY=your_brave_search_key
```

### 3. Configuration File
The main configuration is in `/config/mcpo-config.json` with these MCP servers:

#### Available MCP Servers:
- **Memory**: In-memory storage and retrieval
- **Time**: Date and time operations
- **Filesystem**: File system operations (sandboxed to /tmp)
- **GitHub**: GitHub repository integration
- **Slack**: Slack workspace integration  
- **PostgreSQL**: Database operations
- **Brave Search**: Web search capabilities
- **Puppeteer**: Web scraping and automation
- **SQLite**: Local database operations
- **Everything**: File search and indexing

### 4. Deployment Commands

#### Manual Deployment:
```bash
# Using config file (recommended)
mcpo --host 0.0.0.0 --port 8000 --config /app/config/mcpo-config.json

# Single MCP server example
mcpo --host 0.0.0.0 --port 8000 -- uvx mcp-server-time --local-timezone=UTC

# With authentication
mcpo --host 0.0.0.0 --port 8000 --api-key "your-api-key" --config /app/config/mcpo-config.json
```

#### Docker Compose (automated):
The included `docker-compose.yaml` handles production deployment with:
- Health checks
- Proper networking
- Volume mounts
- Auto-restart policies

## OpenAPI Integration

### Generated Endpoints
MCPO automatically generates OpenAPI-compliant endpoints for each MCP server:

```
https://mcpo.ignitabull.org/docs              # Main documentation
https://mcpo.ignitabull.org/memory/docs       # Memory server docs
https://mcpo.ignitabull.org/time/docs         # Time server docs
https://mcpo.ignitabull.org/github/docs       # GitHub server docs
... (one for each configured server)
```

### OpenAPI Features:
- **Auto-generated schemas** from MCP tool definitions
- **Interactive Swagger UI** for testing
- **JSON/YAML spec export** for integration
- **Type-safe request/response models**
- **Authentication support** (API key)

## MCP Server Management

### Adding New MCP Servers
1. Edit `config/mcpo-config.json`
2. Add server configuration:
```json
{
  "mcpServers": {
    "your_server": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-your-server"],
      "env": {
        "YOUR_API_KEY": "value"
      }
    }
  }
}
```
3. Redeploy application

### Server Types Supported:
- **stdio**: Standard process communication
- **sse**: Server-Sent Events HTTP transport
- **streamable_http**: Streamable HTTP transport

### Testing MCP Connections
Use the health check endpoints:
```bash
curl https://mcpo.ignitabull.org/docs
curl https://mcpo.ignitabull.org/memory/docs
```

## Authentication & Security

### API Key Authentication
```bash
# Set API key in Coolify environment
MCPO_API_KEY=your-secure-api-key

# Or pass via command line
mcpo --api-key "your-api-key" --config config.json
```

### Usage with Authentication:
```bash
# API requests
curl -H "Authorization: Bearer your-api-key" https://mcpo.ignitabull.org/memory

# Basic auth also supported
curl -u "user:your-api-key" https://mcpo.ignitabull.org/memory
```

### Strict Authentication Mode:
Enable to protect documentation pages:
```bash
mcpo --api-key "key" --strict-auth --config config.json
```

## Monitoring & Troubleshooting

### Health Checks
- **Application health**: `GET /docs`
- **Individual servers**: `GET /{server}/docs`
- **Coolify monitoring**: Built-in dashboard

### Common Issues:

#### 1. Module Import Errors
```
/opt/venv/bin/python: No module named mcpo.__main__
```
**Solution**: Ensure `src/mcpo/__main__.py` exists (fixed in current version)

#### 2. MCP Server Connection Failures
**Check**:
- Environment variables are set
- Server command is correct
- Network connectivity
- Required dependencies installed

#### 3. OpenAPI Schema Issues
**Verify**:
- MCP server is running
- Tool schemas are valid
- No circular references in schemas

### Logs Access
```bash
# Coolify logs
# Access via Coolify dashboard → Application → Logs

# Container logs
docker logs mcpo-container-name

# Debug mode
mcpo --config config.json --debug
```

## Integration Examples

### Open WebUI Integration
1. Deploy MCPO with this guide
2. In Open WebUI:
   - Go to Settings → Connections  
   - Add OpenAPI server: `https://mcpo.ignitabull.org`
   - Set API key if authentication enabled
   - Select desired MCP tools

### Custom Applications
```python
import httpx

# Initialize client
client = httpx.Client(
    base_url="https://mcpo.ignitabull.org",
    headers={"Authorization": "Bearer your-api-key"}
)

# Call memory server
response = client.post("/memory", json={
    "action": "store",
    "key": "test",
    "value": "Hello World"
})

# Call time server  
response = client.post("/time", json={
    "timezone": "UTC"
})
```

## Performance Optimization

### Resource Limits
Set in Coolify:
- **Memory**: 2GB minimum recommended
- **CPU**: 2 cores for multiple MCP servers
- **Storage**: 10GB for logs and temporary files

### Scaling Considerations
- Each MCP server runs as separate process
- Memory usage scales with number of active servers
- Consider horizontal scaling for high-load scenarios

## Security Best Practices

1. **Use strong API keys** (32+ characters)
2. **Enable strict authentication** for production
3. **Limit CORS origins** to specific domains
4. **Regular security updates** via Coolify auto-deployment
5. **Monitor access logs** for suspicious activity
6. **Use HTTPS only** (enforced by Coolify/Traefik)

## Backup & Recovery

### Configuration Backup
- MCP configuration: `config/mcpo-config.json`
- Environment variables: Coolify export
- Custom modifications: Git repository

### Recovery Process
1. Redeploy from Git repository
2. Restore environment variables
3. Verify MCP server connections
4. Test OpenAPI endpoints

## Support & Resources

- **MCPO Documentation**: [GitHub Repository](https://github.com/open-webui/mcpo)
- **MCP Protocol**: [Model Context Protocol](https://modelcontextprotocol.io/)
- **Coolify Documentation**: [Coolify Docs](https://coolify.io/docs)
- **Issue Tracking**: GitHub Issues in mcpo repository

---

## Quick Commands Reference

```bash
# Deploy with config file
mcpo --config /app/config/mcpo-config.json

# Single server deployment
mcpo --port 8000 -- uvx mcp-server-time

# With authentication
mcpo --api-key "key" --config config.json

# Debug mode
mcpo --config config.json --debug

# Health check
curl https://mcpo.ignitabull.org/docs
```

This deployment is now production-ready with full OpenAPI compatibility and comprehensive MCP server support.
