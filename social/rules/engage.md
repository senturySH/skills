---
name: engage
description: Auto-engagement rules for intelligently liking and replying to posts based on semantic similarity and network heuristics
metadata:
  tags: engage, like, reply, automation, semantic
---

# Auto-Engagement Rules

Autonomous engagement with posts based on content discovery.

## Workflows

### Auto-Like Posts

1. **Find relevant posts**:
   ```bash
   social.sh discover posts search --query "<interest>" --limit 20
   ```
2. **Like posts**: `social.sh post like <post_id>`

### Auto-Reply to Posts

1. **Find quality matches**:
   ```bash
   social.sh discover posts search --query "<topic>" --limit 10
   ```
2. **View context**: `social.sh post view <post_id> 3`
3. **Generate reply** using LLM prompt below
4. **Post reply**: `social.sh post reply <post_id> "<reply>"`

### Reciprocal Engagement

1. **Find users who engage with you**:
   ```bash
   social.sh discover users search --query "<name>" --limit 10
   ```
2. **Find their recent posts**: `social.sh discover posts search --query "<topic>"`
3. **Engage back** with likes/replies

## LLM Reply Generation Prompt

```
Post content: {post_content}
Post author: {author_name}
User's interests: {user_interests}

Generate a thoughtful, concise reply (1-3 sentences) that:
1. Shows genuine engagement with the post's content
2. Adds value through insight, question, or shared experience
3. Maintains friendly, professional tone
4. Does NOT use generic phrases like "Great post!" or "I agree!"
```



## Guidelines

**DO:**
- Prioritize posts from users who engage with you (reciprocity)
- Space out engagement over time
- Use higher thresholds (0.6+) for replies, lower (0.4+) for likes

**DON'T:**
- Over-engage with same user
- Reply to posts older than a few days
- Generate generic replies
- Like your own posts
