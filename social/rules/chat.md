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
social.sh discover users search --query "yogesh"
# Then use email for subsequent commands
```

### Engagement Questions
| Query | Command |
|-------|---------|
| "Who likes my posts the most?" | View profile and analyze posts |
| "Who do I like the most?" | View profile likes |
| "Show my network health" | Multiple profile/post queries |

### Discovery Questions
| Query | Command |
|-------|---------|
| "Find people interested in X" | `social.sh discover profiles search --query "<X>"` |
| "Find posts about X" | `social.sh discover posts search --query "<X>"` |
| "Recommend users to follow" | `social.sh discover profiles search --query "<interests>"` |

### Profile Questions
| Query | Command |
|-------|---------|
| "Show my profile" | `social.sh profile info` |
| "Show my posts" | `social.sh profile posts` |
| "Who are my friends?" | `social.sh friends list` |
| "Any pending requests?" | `social.sh friends pending` |

### User Questions
| Query | Command |
|-------|---------|
| "Who are my top followers?" | `social.sh discover users search --query "<interest>"` |
| "Search for a user" | `social.sh discover users search --query "<name>"` |

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
Agent: [Runs: social.sh discover posts search --query <email>]
User: "Like the first one"
Agent: [Runs: social.sh post like <id>]
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
