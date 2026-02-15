---
name: network
description: Network growth and analysis rules using heuristic queries and graph traversal
metadata:
  tags: network, analysis, growth, influence, connections, heuristics
---

# Network Analysis Rules

Analyze network patterns, connections, and growth using heuristic queries.

## Key Concepts

- **Incoming** (followers/received): Users who follow you or engage with YOUR content
- **Outgoing** (following/given): Users you follow or YOUR engagement with others
- **Heuristics**: age, proximity, likes, replies (list = individual, count = aggregated)

## Workflows

### Identify Top Engagers

```bash
# Who engages with you most
network heuristic query likes count received --limit 10
network heuristic query replies count received --limit 10

# Individual interactions
network heuristic query likes list received --limit 20
```

### Analyze Your Engagement Patterns

```bash
# Who you engage with most
network heuristic query likes count given --limit 10
network heuristic query replies count given --limit 10
```

Compare incoming vs outgoing to identify reciprocity gaps.

### Network Health Analysis

```bash
friends list              # Current connections
friends pending           # Potential growth
friends sent              # Awaiting acceptance

# Engagement distribution
network heuristic query likes count received --limit 20
network heuristic query likes count given --limit 20
```

Calculate:
- **Engagement Reciprocity**: % of engagers you engage back with
- **Concentration Risk**: Over-reliance on few users
- **Growth Potential**: Pending requests, new connections

### Find Bridge Nodes (Connectors)

```bash
# Graph by engagement strength
network heuristic traverse likes given --depth 2 --width 10
network heuristic traverse replies given --depth 2 --width 10
```

Users in multiple branches with many connections are potential bridges.

### Geographic Analysis

```bash
network heuristic query proximity followers --limit 15
network heuristic traverse proximity followers --depth 2 --width 10
```

### Account Maturity Analysis

```bash
network heuristic query age followers --limit 15
network heuristic traverse age followers --depth 2 --width 8
```

### Network Growth Strategy

1. Accept pending requests from relevant users
2. Reciprocate with top engagers
3. Send friend requests to semantically aligned users
4. Engage with bridge nodes to expand reach

## Network Health Report Template

```
Connections: X friends, Y pending, Z sent
Engagement Received: X likes, Y replies from Z users
Engagement Given: X likes, Y replies to Z users
Reciprocity Score: X%

Top Engagers:
1. user@example.com - 15 likes, 3 replies

Recommendations:
- Reciprocate with [user] (engages but you don't)
- Accept request from [user] (strong semantic match)
```

## Guidelines

**DO:**
- Compare incoming vs outgoing for reciprocity
- Use multiple metrics (likes + replies)
- Consider account age for connection quality
- Suggest actionable next steps

**DON'T:**
- Focus only on quantity
- Ignore geographic patterns
- Over-optimize at expense of authentic relationships
