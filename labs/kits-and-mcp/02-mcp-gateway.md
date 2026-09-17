# The MCP Gateway

Kits cover the environment. What they don't cover is a live connection to
another system — a wiki, a tracker, an internal API. That's MCP, and in a
sandbox you never configure it inside the sandbox.

Instead `sbx` on your **host** keeps the servers and their credentials, and
gives the agent exactly one MCP endpoint: the **gateway**.

## Register a server on the host

Clean up after the last section first — leave the agent session, then throw the
sandbox away:

```prompt terminal-id=sandbox
/exit
```

```bash terminal-id=host
sbx rm demo
```

Now register a server. Its API key goes in as a **placeholder**:

```bash terminal-id=host
sbx mcp add acme --url https://mcp.acme.com/mcp --header 'Authorization: Bearer ${api-key}'
```

Supply the value — `sbx secret set` prompts for it and masks what you type:

```bash terminal-id=host
sbx secret set mcp:acme:api-key
```

Type anything (it's simulated) and press Enter. The registration is a catalog on
your host, not a per-sandbox setting:

```bash terminal-id=host
sbx mcp ls
```

## Hand it to a sandbox

`--static-mcp` names the servers a sandbox may reach, chosen at creation:

```bash terminal-id=sandbox
sbx run claude --name demo --static-mcp acme
```

The gateway starts and attaches the set — note the header line: `Bearer ****`,
`injected on the host`. Now use it:

```prompt terminal-id=sandbox
Search Acme for the on-call runbook.
```

The `→ Calling MCP tool` block is the agent going through the gateway. It asked
for `acme.search`; the gateway forwarded the call and wrote your API key into
the header on the way out. So ask the obvious question:

```prompt terminal-id=sandbox
Do you have my Acme API token?
```

It doesn't, and it can't — which is the whole reason for brokering the
connection instead of configuring MCP inside the sandbox. A leaky agent or a
prompt injection finds one endpoint and no credentials.

Clean up:

```prompt terminal-id=sandbox
/exit
```

```bash terminal-id=host
sbx rm demo
```

> Omitting `--static-mcp` gives the agent the gateway's own discovery tools
> (`mcp-find`, `mcp-add`) instead, so it can attach a server you've registered
> when a task needs one.
