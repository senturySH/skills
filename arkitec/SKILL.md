---
name: arkitec
description: Create and manage subagents through the orchestrator MCP. Use the directory parameter to control hierarchy.
triggers:
  - create agent
  - create subagent
  - add agent to org
  - spawn agent
  - build organization
  - new agent
  - delete agent
  - remove agent
---

# Orchestrator: Subagent Management

You have access to the orchestrator MCP server. It lets you create agents, connect them to the network, equip them with skills, and communicate with them.

**Always call `get_organization` first** before creating anything. Understand what already exists.

All `directory` parameters are **relative** to the workspace root.

## Tool Overview

| Tool                         | Purpose                                              |
|------------------------------|------------------------------------------------------|
| `get_organization`           | List all projects and the full agent tree             |
| `create_agent`               | Create a new agent inside a parent directory          |
| `delete_agent`               | Recursively delete an agent and its subdirectory      |
| `connect_agent`              | Register an agent on the network for messaging        |
| `search_skills`              | Search the skill registry by keyword                  |
| `install_skill`              | Install a skill into an agent's directory             |
| `send_message`               | Send a message to an agent by email                   |
| `get_inbox`                  | List inbox conversations                              |
| `get_conversation`           | Fetch a conversation by id                            |
| `get_conversation_with_user` | Fetch a conversation by user email                    |
| `get_messages`               | Fetch messages in a conversation                      |
| `get_unread_count`           | Get total unread message count                        |
| `mark_conversation_read`     | Mark a conversation as read                           |
| `delete_conversation`        | Delete a conversation                                 |
| `delete_message`             | Delete a single message                               |

## How hierarchy works

The `directory` parameter in `create_agent` controls where an agent lives — and that determines who orchestrates it. The parent directory is the orchestrator.

### Creating a top-level agent

```
create_agent({
  name: "frontend-lead",
  description: "Owns all frontend work",
  directory: "core-platform"
})
```

Result:

```
core-platform/
└── frontend-lead/
```

`core-platform` is the orchestrator of `frontend-lead`.

### Creating a subagent inside another agent

Pass the parent agent's relative path as `directory`:

```
create_agent({
  name: "react-dev",
  description: "Implements React components and pages",
  directory: "core-platform/frontend-lead"
})
```

Result:

```
core-platform/
└── frontend-lead/
    └── react-dev/
```

### Building a full team

```
create_agent({ name: "frontend-lead",  ..., directory: "core-platform" })
create_agent({ name: "react-dev",      ..., directory: "core-platform/frontend-lead" })
create_agent({ name: "css-dev",        ..., directory: "core-platform/frontend-lead" })
```

Result:

```
core-platform/
└── frontend-lead/          ← orchestrates react-dev and css-dev
    ├── react-dev/
    └── css-dev/
```

### Flat vs nested

**Flat** — all agents are peers:

```
create_agent({ name: "api-agent",      ..., directory: "core-platform" })
create_agent({ name: "frontend-agent", ..., directory: "core-platform" })
create_agent({ name: "infra-agent",    ..., directory: "core-platform" })
```

**Nested** — leads orchestrate their own teams:

```
create_agent({ name: "api-lead",       ..., directory: "core-platform" })
create_agent({ name: "auth-agent",     ..., directory: "core-platform/api-lead" })
create_agent({ name: "frontend-lead",  ..., directory: "core-platform" })
create_agent({ name: "react-dev",      ..., directory: "core-platform/frontend-lead" })
```

The rule: **the `directory` you pass is the parent. The parent is the orchestrator.**

## Deleting agents

Recursive — deleting a parent removes all its children:

```
delete_agent({ directory: "core-platform/frontend-lead/css-dev" })  // deletes just css-dev
delete_agent({ directory: "core-platform/frontend-lead" })          // deletes frontend-lead + all children
```

## Skills

```
search_skills({ query: "react components" })
install_skill({ skillId: "component-gen", directory: "core-platform/frontend-lead/react-dev" })
```

## Messaging

Use the `email` from the org tree to communicate with agents.

```
send_message({
  recipientEmail: "react-dev@platform.arkitec",
  content: "Build the dashboard page using the new design tokens"
})
```

Other inbox tools: `get_inbox`, `get_conversation`, `get_conversation_with_user`, `get_messages`, `get_unread_count`, `mark_conversation_read`, `delete_conversation`, `delete_message`.

## Examples

### Example A: Adding a team under an existing agent

**Step 1 — Survey:**

```
get_organization({ reason: "Check existing agents before adding subagents" })
```

**Step 2 — Create subagents using the `directory` from the response:**

```
create_agent({
  name: "react-dev",
  description: "Implements React components, pages, and client-side routing",
  directory: "core-platform/frontend-lead"
})
```

**Step 3 — Connect to network:**

```
connect_agent({ directory: "core-platform/frontend-lead/react-dev" })
```

### Example B: Deciding not to create

If an existing agent already covers the responsibility, message it instead:

```
send_message({
  recipientEmail: "api@platform.arkitec",
  content: "Add request validation middleware for all POST/PUT endpoints using zod schemas"
})
```

### Example C: Deleting agents

```
delete_agent({ directory: "content-lead/copywriter" })
```
