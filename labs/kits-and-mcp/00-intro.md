# Two ways to equip an agent

A Docker Sandbox starts deliberately plain: an agent, your workspace, and a
network policy that allows only what has been approved. A plain agent isn't a
very useful one, though — it needs the tools your project uses and, sometimes, a
live connection to another system.

There are two ways to provide that:

| Feature         | What it is                                                                      |
| --------------- | ------------------------------------------------------------------------------- |
| **Kit**         | A YAML artifact adding tools, files, env vars, network rules and agent memory   |
| **MCP Gateway** | One MCP endpoint inside the sandbox, backed by servers you register on your host |

Either way, the credentials stay **on your host** — the agent uses them without
ever holding them.

The right-hand pane has two tabs, and the split is the point:

- **Host** — where `sbx` runs, and where secrets live.
- **Sandbox** — where the agent runs. It sees your workspace, whatever a kit
  installed, and one MCP endpoint. Nothing else.

> Everything here is **simulated**: every command is matched to author-scripted
> output and runs in your browser. The commands, flags and output are real; the
> execution is not.
