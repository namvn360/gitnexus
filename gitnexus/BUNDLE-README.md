## GitNexus Headless Docker Bundle

This folder is a portable subset of the main repository for three purposes:

- AI-facing skills and editor integrations
- Repository graphing and code-intelligence engine
- Headless Docker-based MCP/server deployment

### Included

- `gitnexus/`: CLI, MCP server, HTTP API, ingestion pipeline, graph/search logic
- `gitnexus-shared/`: shared graph and scope-resolution types
- `gitnexus-claude-plugin/`, `gitnexus-cursor-integration/`: editor/plugin packaging
- `.agents/`, `.claude/`: skill definitions and agent-facing instructions
- `docker-compose.yaml`, `Dockerfile.cli`
- Core docs: `README.md`, `ARCHITECTURE.md`, `RUNBOOK.md`, `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `GUARDRAILS.md`, `TESTING.md`
- `docs/guides/microservices-grpc.md`

### Run With Docker

Put this folder inside the target repository root, then run from this folder:

```bash
docker compose up -d
```

What happens on startup:

- The container mounts the parent directory (`..`) as `/workspace`
- The first step auto-runs `gitnexus analyze /workspace --skip-git`
- After indexing, the shared GitNexus HTTP server starts on port `4747`

Default port:

- GitNexus server: `4747`

### Shared Server Usage

The compose stack is for the shared headless server. It does not run the web UI.

If the mounted repository changes and you want a fresh graph immediately:

```bash
docker compose restart gitnexus-server
```

To disable startup re-indexing:

```bash
GITNEXUS_AUTO_ANALYZE=false docker compose up -d
```

### MCP Usage Without Local Node

This bundle also keeps the MCP-facing config and skills. For Docker-based MCP, use a `docker run` command that mounts the target repo into `/workspace` and starts `gitnexus mcp`.

The bundled `.mcp.json` files use Docker instead of `npx`, but you may still need to adjust the bind-mount path depending on how your editor launches MCP commands.

### Notes

- This bundle intentionally excludes unrelated evaluation and auxiliary repo content.
- This bundle is now headless: no `gitnexus-web`, no web container, no web-specific runtime files.
- The compose file defaults to the published GitNexus image and mounts the parent repo read-only.
- The Dockerfile is included so you can build a custom server image locally if you want to fork or modify the stack.
