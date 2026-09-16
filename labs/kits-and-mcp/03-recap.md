# Recap

|            | **Kit**                                         | **MCP Gateway**                             |
| ---------- | ----------------------------------------------- | ------------------------------------------- |
| Answers    | "This environment lacks what my project needs"  | "This agent needs to reach another system"  |
| Lives in   | A `spec.yaml` you commit                        | A catalog on your host (`sbx mcp add`)      |
| Applied    | At creation, with `--kit`                       | Per sandbox, with `--static-mcp`            |
| Provides   | Tools, files, env vars, network rules, memory   | Live tool calls to MCP servers              |
| Secrets    | Injected into a header by the host proxy        | Held on the host as `mcp:<server>:<key>`    |

Both answer the same question — "how does an agent get what it needs without
getting my credentials?" — from different directions.

## Next

- **[Kits](https://docs.docker.com/ai/sandboxes/customize/kits/)** — the full
  `spec.yaml` reference, credentials, kit arguments, signing, and source
  allowlisting.
- **[MCP Gateway](https://docs.docker.com/ai/sandboxes/mcp-gateway/)** — OAuth
  servers, local stdio servers, dynamic mode, and governance.

> For the sandbox fundamentals underneath this — isolation, the default-deny
> network policy, granting one host at a time — try the **A Tour of Docker** lab.
