# Kits

A kit is a YAML file that describes what to add to a sandbox. It's applied when
the sandbox is **created**, so every sandbox made from it starts the same way.

## First, without one

Switch to the **Sandbox** tab — the **Run** button on this block targets it:

```bash terminal-id=sandbox
sbx run claude --name demo
```

Read `RESOLVE SETUP`: an agent, a workspace, `kits (none)`. Now ask the agent to
lint the project:

```prompt terminal-id=sandbox
Check this project for lint problems.
```

It can't. And note _why_ installing a linter by hand wouldn't really fix it: the
next sandbox starts bare again, and the agent still wouldn't know which rules
your team enforces.

Leave the session and throw the sandbox away:

```prompt terminal-id=sandbox
/exit
```

```bash terminal-id=host
sbx rm demo
```

## Now with one

The kit for that gap is about a dozen lines — a tool to install, a config file, a
host to allow, and a note for the agent:

```yaml no-run-button
schemaVersion: "2"
kind: mixin
name: ruff-lint

setup:
  install:
    - command: "pip install --quiet ruff"
  files:
    - path: /workspace/ruff.toml
      content: |
        line-length = 100
      onlyIfMissing: true

permissions:
  network:
    allow:
      - docs.astral.sh:443

agentInstructions:
  content: |
    Ruff is installed. Run `ruff check` before committing.
    Shared config lives at `/workspace/ruff.toml`.
```

Pass it with `--kit` (a directory, ZIP, git reference or OCI image):

```bash terminal-id=sandbox
sbx run claude --kit ./ruff-lint/ --name demo
```

An `APPLY KITS` stage reports each thing the kit contributed. Ask the same
question as before, word for word:

```prompt terminal-id=sandbox
Check this project for lint problems.
```

This time it lints — and it never asked which linter to use or where the config
was. That came from `agentInstructions`, which the kit wrote into its memory:

```bash terminal-id=sandbox
!cat kits-memory/ruff-lint.md
```

The tool is really there, too:

```bash terminal-id=sandbox
!ruff check .
```

But only in the sandbox. On the **Host** tab:

```bash terminal-id=host
ruff check .
```

> That's the trade a kit makes: a fully equipped environment for the agent, no
> new tools on your laptop — and it's a file you can commit next to the code.
