# GitNexus: Code Intelligence Engine

Welcome to the **GitNexus** integration for our repository. GitNexus is a powerful, AI-driven code intelligence engine that indexes our codebase to build a comprehensive knowledge graph of symbols, relationships, and execution flows. 

This tool is designed to help developers and AI coding assistants safely navigate complex codebases, perform impact analysis, trace execution paths, and refactor with confidence.

---

## 🚀 Getting Started

GitNexus runs as a headless Docker service, analyzing the repository it is placed in.

### 1. Starting the GitNexus Server

To initialize GitNexus in this repository, navigate to the `gitnexus` directory and start the Docker container:

```bash
cd gitnexus
docker compose up -d
```

**What happens on startup?**
- The container mounts the root of the repository into `/workspace`.
- It automatically analyzes and indexes the code using `gitnexus analyze`.
- A shared GitNexus HTTP server spins up on port **4747**.

### 2. Updating the Index

As you write code and change the repository, you can refresh the knowledge graph immediately by restarting the server:

```bash
cd gitnexus
docker compose restart gitnexus-server
```

To run a targeted index refresh manually, you can also execute `npx gitnexus analyze` from the command line.

---

## 🤖 Model Context Protocol (MCP) Integration

GitNexus seamlessly integrates with modern AI coding assistants (such as Claude Code, Cursor, Windsurf) via the **Model Context Protocol (MCP)**. 

By providing AI agents access to the GitNexus knowledge graph, the AI can make deeply informed decisions rather than relying on standard string searches (grepping). The `.mcp.json` file configures the AI agent to launch the GitNexus MCP server via Docker, granting it read-only access to query the codebase safely without needing a complex local Node.js environment setup.

---

## 🛠️ GitNexus MCP Tools

When interacting with AI assistants in this repository, GitNexus provides the following specialized tools to ensure code safety and deep architectural understanding:

### Discovery & Context
- **`gitnexus_query`**: Finds code by concept or execution flow rather than simple text grep. Ideal for exploring unfamiliar code or debugging specific symptoms.
- **`gitnexus_context`**: Provides a 360-degree view of a specific symbol, including its callers, callees, and the execution flows it participates in.
- **`gitnexus_route_map`**: Maps API routes to their internal handlers and external consumers.

### Safety & Impact Analysis
- **`gitnexus_impact`**: Determines the blast radius of a change before making it. It checks the depth of impact (e.g., direct callers vs. indirect dependencies) to prevent breaking downstream code.
- **`gitnexus_detect_changes`**: A pre-commit check that verifies exactly which symbols and flows are affected by staged changes, ensuring unintended side effects are caught early.
- **`gitnexus_api_impact`**: Pre-assesses the impact of changing an API route (e.g., altering a response shape) on downstream consumers.

### Refactoring & Advanced Operations
- **`gitnexus_rename`**: Performs a safe, graph-aware multi-file rename, distinguishing between actual code references and unrelated text matches.
- **`gitnexus_shape_check`**: Validates response shapes against consumer access patterns.
- **`gitnexus_cypher`**: Allows executing custom graph queries (using Cypher syntax) directly against the codebase index for advanced architectural audits.

---

## ⚠️ Best Practices & Guardrails

To ensure we maintain a stable codebase, please adhere to the following when using AI assistants or developing locally:

1. **Always Check Impact:** Never modify a function, class, or method without running an impact analysis (`gitnexus_impact`).
2. **Review Blast Radius:** If GitNexus reports a HIGH or CRITICAL risk level for a proposed change, carefully review the dependencies before proceeding.
3. **Avoid Blind Grepping:** Use `gitnexus_query` and `gitnexus_context` to understand the code rather than relying on generic string searches.
4. **Pre-commit Verification:** Ensure `gitnexus_detect_changes()` is utilized to validate the scope of modifications prior to finalizing a commit.

*For more detailed architecture, runbooks, and testing information, refer to the documentation inside the `gitnexus/` directory (e.g., `AGENTS.md`, `ARCHITECTURE.md`).*
