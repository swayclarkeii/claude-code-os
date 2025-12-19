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

## Workflow Optimization Patterns

**Applies to:** Make.com, N8N, Zapier, GitHub Actions, any workflow platform

### The #1 Optimization: Early Filtering

**Problem:** Processing all items through expensive operations before filtering

```
❌ INEFFICIENT (what most people build):
Raw Data (100 items) → API Call (100 ops) → Normalize (100 ops) → Filter → 10 results
= 200 wasted operations

✅ OPTIMIZED (what you should build):
Raw Data (100 items) → Filter → 10 items → API Call (10 ops) → Normalize (10 ops)
= 20 operations (90% savings!)
```

**Key insight:** Move filters BEFORE expensive operations, not after.

**Typical savings:** 40-70% reduction in operations/costs

**Implementation time:** 1-2 hours

**Risk:** Zero (if verification process followed)

### Three Core Patterns

#### Pattern 1: Early Filtering
Move basic filters BEFORE expensive operations.

**When to use:**
- Filter criteria don't require expensive calculations
- Most items will be filtered out
- Expensive operations (API calls, normalizations) happen after filter

**Example:**
```
Location filter → URL keyword filter → Status filter
↓ (10 items remain)
API enrichment → Price normalization → Complex calculations
```

#### Pattern 2: Redundant Module Removal
Remove modules that don't affect downstream logic.

**How to identify:**
- Check if module output is referenced by other modules
- Verify if module has side effects (writes, emails, etc.)
- If neither, it's likely redundant

**Impact:** Reduces maintenance complexity + operation count

#### Pattern 3: Filter Logic Splitting
Split complex filters into early and late stages.

**Structure:**
```
Early stage: Basic criteria (location, keywords, status)
Late stage: Calculated criteria (price after normalization, API-derived data)
```

**Why it works:** Process fewer items through expensive calculations

### Optimization Methodology

**4-Step Process:**

1. **Map the flow** - List all modules, count operations at each step
2. **Identify bottlenecks** - Where are most operations happening?
3. **Calculate savings** - What if we reorder operations?
4. **Verify results** - PROVE changes don't affect output

### Verification Framework

**CRITICAL:** Always verify optimizations don't change results

**Process:**
1. Export results BEFORE optimization
2. Export results AFTER optimization
3. Compare counts (must match)
4. Spot check data (must match)
5. Prove filter equivalence mathematically

**Mathematical Equivalence Example:**
```
ORIGINAL: Items → Normalize → Filter(A AND B AND C)
OPTIMIZED: Items → Filter(A AND B) → Normalize → Filter(C)

PROOF:
Let A = location filter, B = keyword filter, C = price filter
Original: (A AND B AND C) after normalization
Modified: (A AND B) then (C) after normalization

Since A and B are independent of normalization:
Result is mathematically identical ✓
```

### Platform-Specific Notes

#### Make.com
- **Blueprint structure:** Check for nested Router modules (`gateway:CustomRouter`)
- **Module references:** Different routes use different IDs (`{{23.field}}` vs `{{232.field}}`)
- **IML filter syntax:** `{"and": true, "conditions": [...]}`
- **Python automation:** For 10+ module changes, write script to handle nested structures

#### N8N
- Same optimization patterns apply
- Filter syntax different (node-based, not IML)
- Code nodes allow inline JavaScript for complex filters
- Will document N8N-specific syntax when building N8N projects

#### GitHub Actions
- Optimization = conditional steps (`if:` clauses)
- Filter early in workflow to skip expensive steps
- Use matrix strategies wisely (multiplicative effect)

### Real-World Results

**Lombok Capital Case Study:**
- Before: 555 credits per run
- After: 240 credits per run
- **Savings: 56% (315 credits)**
- Time to optimize: 2 hours
- Verification: ✓ Same 15 results before/after

**Key lesson:** One optimization pays for itself immediately, benefits compound forever

### Client Communication

**Present in business terms:**
❌ Don't say: "282 operations on normalize module creating inefficiency"
✅ Do say: "We can reduce costs from $X to $Y per run - 56% savings with zero risk"

**Always include:**
- Current cost/operations
- Optimized cost/operations
- % savings
- Risk assessment
- Verification plan

### Quick Decision: When to Optimize?

**YES, optimize when:**
- Workflow runs frequently (>10 times/month)
- Contains expensive operations
- Processing 50+ items through sequential steps
- Budget/cost is a client concern

**NO, skip when:**
- Workflow runs infrequently (<10 times/month)
- Operations already low (<50 operations)
- No expensive operations in flow
- Client requests fastest build time

---

## Remember

1. **Check docs first** - Always verify functions exist before recommending
2. **Cost matters** - Make.com operations add up fast; optimize BEFORE building
3. **Client handoff** - If client needs to maintain, prioritize simplicity
4. **Start simple** - Don't over-engineer; complexity can be added later
5. **Lombok lesson** - When HTML parsing + memory needs exceed Make.com limits, go custom
6. **Filter early** - Move filters BEFORE expensive operations (40-70% typical savings)
7. **Verify always** - Prove optimizations don't change results before deploying
