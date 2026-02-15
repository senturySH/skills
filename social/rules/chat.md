---
name: chat
description: Conversational interface rules for natural language queries about your social network
metadata:
  tags: chat, conversational, natural-language, queries, assistant
---

# Conversational Interface Rules

Translate natural language questions into CLI commands and present results conversationally.

## Intent Mapping

### User Resolution (Name → Email)
When users refer to people by name, resolve first:
```bash
social_dev discover users search --query "yogesh"
# Then use email for subsequent commands
```

### Engagement Questions
| Query | Command |
|-------|---------|
| "Who likes my posts the most?" | `network heuristic query likes count received` |
| "Who do I like the most?" | `network heuristic query likes count given` |
| "Who replies to me?" | `network heuristic query replies count received` |
| "Who do I reply to?" | `network heuristic query replies count given` |
| "Am I reciprocating?" | Compare received vs given |

### Discovery Questions
| Query | Command |
|-------|---------|
| "Find people interested in X" | `network semantic search musings following "<X>"` |
| "Find posts about X" | `network semantic search posts following "<X>"` |
| "Recommend users to follow" | `network semantic traverse discovery "<interests>"` |

### Profile Questions
| Query | Command |
|-------|---------|
| "Show my posts" | `profile posts` |
| "What are my musings?" | `profile musings` |
| "Who are my friends?" | `friends list` |
| "Any pending requests?" | `friends pending` |

### Network Questions
| Query | Command |
|-------|---------|
| "Who are my closest followers?" | `network heuristic query proximity followers` |
| "Who are my oldest followers?" | `network heuristic query age followers` |
| "Show my network health" | Multiple queries (see network.md) |

## Response Formatting

**List results**: Numbered list with relevant details, then offer follow-up actions.

**Analysis results**: Key insights, opportunities, and suggested actions.

**Search results**: Matches with similarity scores and suggested next steps.

## Ambiguous Queries

Ask for clarification:
```
"Show me engagement" → 
1. Who engages with you
2. Who you engage with  
3. Full analysis
Which would you like?
```

## Follow-up Handling

Support conversational context:
```
User: "Who likes my posts the most?"
Agent: [Shows top 5]
User: "Show me the first one's posts"
Agent: [Runs: discover posts --user <email>]
User: "Like the first one"
Agent: [Runs: post like <id>]
```

## Guidelines

**DO:**
- Offer follow-up actions
- Present data in human-readable format
- Ask for clarification on ambiguous queries
- Remember context from previous exchanges

**DON'T:**
- Dump raw command output
- Execute destructive actions without confirmation
- Overwhelm with too much data
