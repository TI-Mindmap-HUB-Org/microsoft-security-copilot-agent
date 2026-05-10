# TI Mindmap Hub — Microsoft Security Copilot Agent

A Microsoft Security Copilot agent that integrates directly with the [TI Mindmap Hub](https://ti-mindmap-hub.com/) REST API for threat intelligence capabilities.

## Architecture

```
┌──────────────────────────┐                    ┌──────────────────────────┐
│  Microsoft Security      │   REST + API Key   │  TI Mindmap Hub         │
│  Copilot                 │ ─────────────────> │  Backend REST API       │
│                          │   (X-API-Key)      │                         │
│  • manifest.yaml         │                    │  • Reports              │
│  • openapi.yaml (3.0.1)  │                    │  • Briefings            │
│  • API Key auth          │                    │  • IOC Search           │
│                          │                    │  • CVE Intelligence     │
│                          │                    │  • STIX Bundles         │
│                          │                    │  • Knowledge Graph      │
└──────────────────────────┘                    └──────────────────────────┘
```

**No proxy required** — Security Copilot calls the TI Mindmap Hub REST API directly via OpenAPI spec.

## Available Skills (22 endpoints)

| Category | Endpoints | Description |
|----------|-----------|-------------|
| **Reports** | 4 | List, search, get details, sources, and tags |
| **Weekly Briefings** | 3 | Latest, list all, and get by date |
| **IOC Search** | 1 | Search IOCs across all processed reports |
| **CVE Intelligence** | 5 | Search by ID/keyword, list, by-article, and statistics |
| **STIX Bundles** | 3 | Get bundle, list all, and statistics |
| **Statistics & Submissions** | 2 | Platform stats and article submission |
| **Knowledge Graph** | 6 | Search, cluster, timeline, attack-path, cross-report, stats |

## Authentication

**API Key** via `X-API-Key` header — directly to TI Mindmap Hub.

### Getting an API Key

1. Sign up at [ti-mindmap-hub.com](https://ti-mindmap-hub.com/)
2. Go to **My Profile → MCP Server API Keys**
3. Click **Generate Key**
4. Copy your key (format: `tim_xxxxxxxxxxxx`)

## Project Structure

```
security-copilot-ti-mindmap-hub-agent/
├── manifest.yaml         # Full agent manifest (AGENT + API + GPT skills + AgentDefinitions)
├── plugin-apikey.yaml     # Simplified API-only plugin (no orchestrator)
├── openapi.yaml           # OpenAPI 3.0.1 spec (host publicly for Security Copilot)
└── README.md
```

## Manifest Options

| File | Use Case |
|------|----------|
| `manifest.yaml` | Full agent with AGENT orchestrator, GPT triage skill, and automated monitoring trigger |
| `plugin-apikey.yaml` | Lightweight — API skills only, no orchestrator |

## Quick Start

### 1. Upload to Security Copilot

The OpenAPI spec is already hosted on this repo at:
```
https://raw.githubusercontent.com/TI-Mindmap-HUB-Org/microsoft-security-copilot-agent/main/openapi.yaml
```

Both `manifest.yaml` and `plugin-apikey.yaml` are pre-configured to use this URL — no changes needed.

### 2. Upload to Security Copilot

1. Sign in to [Security Copilot](https://securitycopilot.microsoft.com/)
2. Click **Sources** → **Manage plugins**
3. Scroll to **Custom** → **Add plugin**
4. Select **Security Copilot plugin** → Upload `manifest.yaml`
5. When prompted, enter your TI Mindmap Hub API key (`tim_xxxxxxxxxxxx`)
6. Enable the plugin

### 3. Use It

Example prompts:

```
List recent threat intelligence reports about ransomware
Search for IOC 185.220.101.1 in TI Mindmap Hub
What is CVE-2024-3400?
Show me the latest weekly threat briefing
Find threat actors connected to APT29 in the knowledge graph
Map the attack path for T1566 (spearphishing)
What entities are shared between these two reports?
```

## Agent Features

### Threat Intelligence Analyst (AGENT skill)
Orchestrates multi-step investigations by chaining API calls. For example:
1. Search for a threat actor in the Knowledge Graph
2. Get the ego-graph cluster to find related malware/TTPs
3. Look up associated CVEs
4. Synthesize findings into an actionable summary

### IOC Triage Summary (GPT skill)
Formats raw IOC search results into analyst-ready triage reports with risk assessment and recommended actions.

### Automated Threat Monitor (AgentDefinition)
Periodically fetches new reports (default: every 60 minutes) and processes them through the analyst agent for proactive threat awareness.

## References

- [TI Mindmap Hub Docs](https://docs.ti-mindmap-hub.com/)
- [TI Mindmap Hub MCP](https://docs.ti-mindmap-hub.com/mcp/)
- [Security Copilot Custom Plugins](https://learn.microsoft.com/en-us/copilot/security/custom-plugins)
- [Security Copilot Agent Manifest](https://learn.microsoft.com/en-us/copilot/security/developer/agent-manifest)
- [Security Copilot API Plugin](https://learn.microsoft.com/en-us/copilot/security/plugin-api)

## Repository

[github.com/TI-Mindmap-HUB-Org/microsoft-security-copilot-agent](https://github.com/TI-Mindmap-HUB-Org/microsoft-security-copilot-agent)

## License

[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)
