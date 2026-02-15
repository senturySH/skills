---
name: post
description: Posting assistant rules for generating, refining, creating posts, and workflow-driven musing generation
metadata:
  tags: post, create, generate, musing, content, llm, workflow
---

# Posting Assistant Rules

Create posts and musings with AI assistance, including workflow-driven auto-generation.

## Posts vs Musings

| Aspect | Posts | Musings |
|--------|-------|---------|
| Purpose | Share updates, discussions | Define interests for discovery |
| Visibility | Appears in feeds | Used for semantic matching |
| Engagement | Can be liked/replied | Not directly engaged with |
| Creation | Manual or assisted | Often auto-generated from workflow |

## Core Commands

```bash
post create "<content>"       # Create post
muse create "<content>"       # Create musing
profile posts                 # View your posts
profile musings               # Check existing musings
```

## Workflow-Driven Musing Generation (PROACTIVE)

**Auto-suggest musings** when observing user work patterns:

### Trigger Signals

| Signal | Suggested Musing |
|--------|------------------|
| Rust files edited | "Building systems in Rust, embracing ownership" |
| React/Next.js work | "Crafting UX with React and Next.js" |
| Docker/K8s configs | "Cloud-native infrastructure and containerization" |
| SQL/pandas work | "Exploring data, finding stories in numbers" |
| Test files | "Advocate for test-driven development" |

### Notification Format

```
[social.sh] Musing Suggestion
I noticed you've been working on {pattern}.
Would you like me to create a musing about this?

Suggested: "{musing_content}"

[y] Create  [n] Skip  [e] Edit
```

### Before Suggesting

Always check `profile musings` to avoid duplicates.

### Frequency

| Scenario | Frequency |
|----------|-----------|
| New technology | Once when pattern is clear |
| Deepening interest | Weekly if significant progress |
| Same topic | Don't re-suggest |
| User declined | Wait 1 week |

## Workflow: Generate Post Ideas

1. **Check interests**: `profile musings` and `profile posts`
2. **Analyze trends**: `discover feed`
3. **Generate ideas** using LLM prompt below

## Workflow: Create Post

1. Get topic/draft from user
2. Generate content with LLM
3. Present draft with options (post, shorten, make casual, add question)
4. Execute: `post create "<content>"`

## LLM Post Generation Prompt

```
Generate a social media post about: {topic}

Requirements:
- Concise (1-3 sentences, max 280 chars preferred)
- Engaging hook, insight, or question
- Authentic voice, not corporate
- Optionally end with question for replies

Tone: {casual/professional/humorous/thoughtful}
```

## LLM Musing Generation Prompt

```
Observed workflow context:
- Technologies: {technologies}
- Problem domain: {domain}
- Recent activities: {activities}

Generate a musing (1-2 sentences) that:
1. Captures the essence of their work
2. Expresses genuine interest
3. Sounds personal, not like a resume

Good: "Building distributed systems that handle failure gracefully"
Bad: "I use TypeScript and React" (too bland)
```

## Post Templates

**Insight**: `{Observation}. {Context}. {Question?}`

**Question**: `{Thought-provoking question}. {Your take}`

**Hot Take**: `Hot take: {Opinion}. {Brief justification}`

**TIL**: `TIL: {Learning}. {Why it matters}`

## Guidelines

**DO:**
- Keep posts under 280 characters
- Use user's natural voice
- Include attention-grabbing hooks
- End with questions for engagement
- Check existing musings before suggesting

**DON'T:**
- Generate generic/corporate content
- Over-post (quality over quantity)
- Create content outside user's expertise
- Post without user confirmation
