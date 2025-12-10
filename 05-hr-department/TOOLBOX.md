# Automation Toolbox Reference

A decision guide for choosing the right platform when building automation solutions.

---

## Platform Decision Matrix

| Criteria | Make.com | N8N | GitHub Actions | Custom Code |
|----------|----------|-----|----------------|-------------|
| **Cost** | $9-29/mo + operations | Free (self-hosted) | Free (2000 min/mo) | Hosting costs |
| **Complexity** | Low-Medium | Medium-High | Medium | High |
| **Brand Integrations** | Excellent (Apify, etc.) | Good | Limited | Full control |
| **HTML/Memory Intensive** | Limited | Better | N/A | Best |
| **Client Handoff** | Easy | Harder | Medium | Hardest |
| **Sway's Familiarity** | High | Medium | Medium | High |
| **Visual Builder** | Yes | Yes | No (YAML) | No |
| **Error Handling** | Built-in | Built-in | Manual | Manual |

---

## When to Use What

### Make.com
**Best for:**
- Quick client builds with brand integrations (Apify, Airtable, Slack, etc.)
- Visual workflows clients can understand and maintain
- Low complexity, high integration needs
- Projects where client handoff is important

**Avoid when:**
- Memory-intensive operations (HTML parsing, large data)
- Cost is a primary concern (operations add up)
- Need JavaScript/Python for complex logic

**Key integrations:** Apify, Airtable, Google Sheets, Slack, HubSpot, Notion

---

### N8N (Self-Hosted)
**Best for:**
- Cost-sensitive projects (self-hosted = free)
- More complex logic than Make allows
- Projects requiring JavaScript/Python code nodes
- Heavy data transformation
- Internal tools (no client handoff needed)

**Avoid when:**
- Client needs to maintain/modify the workflow
- Quick turnaround needed (steeper learning curve)
- Simple integrations that Make handles better

**Key features:** Code nodes (JS/Python), self-hosted, credential encryption

---

### GitHub Actions
**Best for:**
- Scheduled tasks (cron jobs)
- Code deployment automation
- CI/CD pipelines
- Git-triggered workflows
- Free tier is generous (2000 minutes/month)

**Avoid when:**
- Non-technical triggers needed (webhooks from external services)
- Visual workflow design is important
- Need persistent state between runs

**Key use cases:** Automated testing, deployment, scheduled data pulls

---

### Custom Code
**Best for:**
- Memory-intensive operations (like Lombok HTML parsing)
- Complex algorithms
- When no-code tools hit their limits
- Full control requirements
- Performance-critical applications

**Avoid when:**
- Time is limited
- Client needs to maintain it
- Simple automation that Make/N8N can handle

**Languages:** Python (data), Node.js (web), Go (performance)

---

## Quick Decision Flowchart

```
START: What are you building?
│
├─► Simple integration? ──► Make.com
│
├─► Complex logic needed?
│   ├─► Client will maintain? ──► Make.com (with workarounds)
│   └─► Internal only? ──► N8N
│
├─► Scheduled/cron task?
│   ├─► Code-related? ──► GitHub Actions
│   └─► Data processing? ──► N8N or Make.com
│
├─► Memory intensive? ──► Custom Code
│
└─► Not sure? ──► Start with Idea Architect Agent
```

---

## Cost Comparison (Monthly)

| Volume | Make.com | N8N | GitHub Actions |
|--------|----------|-----|----------------|
| Light (1k ops) | ~$9 | $0 | $0 |
| Medium (10k ops) | ~$29 | $0 | $0 |
| Heavy (50k+ ops) | $59+ | $0 (hosting ~$5-20) | $0 (within limit) |

---

## MCP Access Reference

### N8N MCP
```
# Search for nodes
mcp__n8n-mcp__search_nodes({query: "webhook"})

# Get full documentation
mcp__n8n-mcp__get_node({nodeType: "nodes-base.webhook", mode: "docs"})

# Validate before building
mcp__n8n-mcp__validate_node({nodeType: "...", config: {...}})
```

### Make.com MCP
```
# Organization ID: 435122
# List scenarios
mcp__make__scenarios_list({teamId: ...})

# Get module info
mcp__make__app-module_get({...})
```

---

## Remember

1. **Check docs first** - Always verify functions exist before recommending
2. **Cost matters** - Make.com operations add up fast
3. **Client handoff** - If client needs to maintain, prioritize simplicity
4. **Start simple** - Don't over-engineer; complexity can be added later
5. **Lombok lesson** - When HTML parsing + memory needs exceed Make.com limits, go custom
