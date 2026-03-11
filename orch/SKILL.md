---
name: arkitec-orchestrator
description: Create and manage subagents through the arkitec orchestrator MCP. Use the directory parameter to control hierarchy.
triggers:
  - create agent
  - create subagent
  - add agent to org
  - spawn agent
  - build organization
  - new agent
---

# Orchestrator: Subagent Management

You have access to the orchestrator MCP server. It lets you create agents, connect them to the network, equip them with skills, and communicate with them.

**Always call `get_organization` first** before creating anything. Understand what already exists.

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

Pass the parent agent's path as `directory`:

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

`frontend-lead` is now the orchestrator of `react-dev`. `core-platform` still orchestrates `frontend-lead`.

### Building a full team

Three calls, each with a different `directory`:

```
create_agent({
  name: "frontend-lead",
  description: "Coordinates frontend agents and owns UI architecture",
  directory: "core-platform"
})
```

```
create_agent({
  name: "react-dev",
  description: "Implements React components and pages",
  directory: "core-platform/frontend-lead"
})
```

```
create_agent({
  name: "css-dev",
  description: "Handles styling, layout, and design system tokens",
  directory: "core-platform/frontend-lead"
})
```

Result:

```
core-platform/
└── frontend-lead/          ← orchestrates react-dev and css-dev
    ├── react-dev/
    └── css-dev/
```

### Flat vs nested — same tool, different directory

**Flat** — all agents are peers under the project:

```
create_agent({ name: "api-agent",      ..., directory: "core-platform" })
create_agent({ name: "frontend-agent", ..., directory: "core-platform" })
create_agent({ name: "infra-agent",    ..., directory: "core-platform" })
```

```
core-platform/              ← orchestrates all three
├── api-agent/
├── frontend-agent/
└── infra-agent/
```

**Nested** — leads orchestrate their own teams:

```
create_agent({ name: "api-lead",       ..., directory: "core-platform" })
create_agent({ name: "auth-agent",     ..., directory: "core-platform/api-lead" })
create_agent({ name: "routes-agent",   ..., directory: "core-platform/api-lead" })

create_agent({ name: "frontend-lead",  ..., directory: "core-platform" })
create_agent({ name: "react-dev",      ..., directory: "core-platform/frontend-lead" })
create_agent({ name: "css-dev",        ..., directory: "core-platform/frontend-lead" })
```

```
core-platform/
├── api-lead/               ← orchestrates auth-agent and routes-agent
│   ├── auth-agent/
│   └── routes-agent/
└── frontend-lead/          ← orchestrates react-dev and css-dev
    ├── react-dev/
    └── css-dev/
```

The rule: **the `directory` you pass is the parent. The parent is the orchestrator.**

## Tools

### get_organization

Always call this first. Returns the full project list and agent tree.

```
get_organization({})
```

Returns:

- **`projects`** — registered project directories with `id`, `path`, `config`
- **`organization`** — tree of `AgentNode` objects

Each `AgentNode`:

| Field         | Type          | Description                              |
|---------------|---------------|------------------------------------------|
| `name`        | string        | Agent name                               |
| `description` | string?       | What this agent does                     |
| `email`       | string?       | Network address for messaging            |
| `directory`   | string        | Absolute path where the agent lives      |
| `skills`      | AgentSkill[]  | Installed skills                         |
| `subAgents`   | AgentNode[]   | Child agents nested under this one       |

### create_agent

Creates an agent inside a directory. The directory determines the parent orchestrator.

| Parameter     | Type   | Required | Description                                    |
|---------------|--------|----------|------------------------------------------------|
| `name`        | string | yes      | Agent name — becomes the subdirectory name     |
| `description` | string | yes      | What this agent is responsible for             |
| `directory`   | string | yes      | Parent directory — this agent's orchestrator   |

### connect_agent

Connects an agent to the network so it can send and receive messages.

```
connect_agent({
  directory: "core-platform/frontend-lead"
})
```

| Parameter   | Type    | Required | Description                          |
|-------------|---------|----------|--------------------------------------|
| `directory` | string  | yes      | The agent's directory                |
| `agentName` | string  | no       | Override the network name            |

### search_skills / install_skill

Find and install skills to give an agent capabilities.

```
search_skills({ query: "react components" })
```

```
install_skill({
  skillId: "component-gen",
  directory: "core-platform/frontend-lead/react-dev"
})
```

### Messaging

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

**Situation:** "Add a react dev and a css dev under frontend-lead."

**Step 1 — Survey:**

```
get_organization({})
```

Response:

```
core-platform/
├── api-agent/
└── frontend-lead/          ← exists, no children yet
```

**Step 2 — Create subagents inside frontend-lead:**

```
create_agent({
  name: "react-dev",
  description: "Implements React components, pages, and client-side routing",
  directory: "core-platform/frontend-lead"
})
```

```
create_agent({
  name: "css-dev",
  description: "Handles styling, CSS modules, and design system tokens",
  directory: "core-platform/frontend-lead"
})
```

**Step 3 — Connect both to network:**

```
connect_agent({ directory: "core-platform/frontend-lead/react-dev" })
connect_agent({ directory: "core-platform/frontend-lead/css-dev" })
```

Result:

```
core-platform/
├── api-agent/
└── frontend-lead/
    ├── react-dev/
    └── css-dev/
```

`frontend-lead` now orchestrates both.

### Example B: Deciding not to create

**Situation:** "Create an agent for API validation."

**Step 1 — Survey:**

```
get_organization({})
```

Response:

```
core-platform/
├── api-agent/          — Handles REST API endpoints and business logic
└── frontend-agent/
```

**Step 2 — Reason:**

API validation is part of API business logic. `api-agent` already owns that boundary. A separate `validation-agent` would split one responsibility across two agents.

**Step 3 — Message the existing agent instead:**

```
send_message({
  recipientEmail: "api@platform.arkitec",
  content: "Add request validation middleware for all POST/PUT endpoints using zod schemas"
})
```
