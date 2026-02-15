---
name: dm
description: Direct messaging rules for managing conversations, outreach, and relationship building
metadata:
  tags: dm, messaging, outreach, conversation, relationships
---

# Direct Messaging Rules

Manage conversations, networking outreach, and relationship building.

## Core Commands

```bash
dm inbox                      # View all conversations
dm chat <email>               # View conversation with user
dm send <email> "<message>"   # Send direct message
dm read <conversation_id>     # Mark as read
```

## Workflows

### Networking Outreach

1. **Find targets**:
   ```bash
   network heuristic query likes count received --limit 10
   network semantic search musings followers "<interest>" --threshold 0.6
   ```
2. **Research**: `discover posts --user <email> --limit 5`
3. **Craft personalized message** referencing their work
4. **Send**: `dm send <email> "<message>"`

### Follow Up on Engagement

1. **Identify high-engagement users**:
   ```bash
   network heuristic query likes count received --limit 5
   ```
2. **Check existing conversations**: `dm inbox`
3. **Reach out if no prior conversation**

### Manage Inbox

1. `dm inbox` - See all conversations with unread counts
2. `dm chat <email>` - Read specific conversation
3. `dm read <id>` - Mark as read
4. `dm send <email> "<response>"` - Respond

## LLM Message Generation Prompt

```
Recipient: {name} ({email})
Recipient's interests: {musings}
Recipient's recent posts: {posts}
Relationship: {new/engaged/friend}
Purpose: {thanks/question/networking/follow-up}

Generate a direct message that:
1. References specific content from recipient
2. Has a clear purpose
3. Is 2-4 sentences max
4. Sounds natural, not corporate
5. Avoids "Hope this finds you well"
```


## Guidelines

**DO:**
- Personalize every message
- Thank users who engage with your content
- Keep messages concise and purposeful

**DON'T:**
- Send cold messages without research
- Send same template to multiple people
- Be pushy or salesy
- Start with generic openers
