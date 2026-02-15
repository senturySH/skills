---
name: discover
description: Content discovery and curation rules using semantic search and network traversal
metadata:
  tags: discover, curate, feed, semantic, users, content
---

# Content Discovery Rules

Find relevant content and users using semantic search and graph traversal.

## Workflows

### Curate Content Feed

1. **Understand interests**: `profile musings` and `profile likes`
2. **Search network**:
   ```bash
   network semantic search musings following "<interest>" --limit 10 --threshold 0.5
   network semantic search posts following "<interest>" --limit 10
   ```
3. **Present curated results** with author info, similarity scores, and actions

### Discover Similar Users

1. **Search by musing similarity**:
   ```bash
   network semantic search musings followers "<interest>" --limit 15 --threshold 0.6
   ```
2. **Graph traversal for deeper discovery**:
   ```bash
   network semantic traverse discovery "<interest>" --depth 2 --width 5 --threshold 0.5
   ```
3. **Verify relevance**: `discover posts --user <email> --limit 5`
4. **Suggest follow**: `friends request <email>`

### Topic Exploration

1. **Broad search**:
   ```bash
   discover posts --query "<topic>" --limit 20
   ```
2. **Graph traversal**:
   ```bash
   network semantic traverse pure "<topic>" --depth 3 --width 8
   network semantic traverse popular "<topic>" --depth 2 --width 10
   ```
3. **Summarize** key themes, notable users, and trending perspectives

### Global User Discovery

Search ALL users by their musing summaries:
```bash
discover musings search --query "<interest>" --limit 10 --threshold 0.3
```
Returns users ranked by semantic similarity to your query.

## Preset Selection

| Goal | Preset |
|------|--------|
| Niche content | `discovery` |
| Trending content | `popular` |
| Balanced exploration | `balanced` |
| Pure topic match | `pure` |
| Trusted sources | `trusted` |

## Guidelines

**DO:**
- Start with musings for user discovery (deeper interests)
- Use posts for topic/content discovery (current discussions)
- Combine multiple search strategies
- Adjust thresholds based on result quality

**DON'T:**
- Suggest following without checking content relevance
- Use only one search method
- Ignore similarity scores
