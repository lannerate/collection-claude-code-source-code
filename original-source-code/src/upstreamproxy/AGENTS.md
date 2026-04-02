<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# upstreamproxy

## Purpose
HTTP/HTTPS proxy configuration for upstream API calls. Handles proxy detection, authentication, and routing for environments that require traffic to flow through a corporate or network proxy (e.g., `HTTP_PROXY`, `HTTPS_PROXY` environment variables).

## Key Files

| File | Description |
|------|-------------|
| `upstreamProxy.ts` | Proxy detection, configuration, and request routing |
| `proxyAgent.ts` | HTTP(S) agent with proxy support for API calls |

## For AI Agents

### Working In This Directory
- Reads `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY` environment variables
- Provides a configured HTTP agent used by the API client in `services/api/`
- Proxy authentication supports Basic auth via the proxy URL
- `NO_PROXY` handling allows bypassing proxy for specific hosts
