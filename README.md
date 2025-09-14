## dnSpyEx (Custom Fork)

This repository is a targeted fork of dnSpyEx focused on two goals:

- Make the codebase build and run smoothly in a modern setup (VS 2022).
- Add an MCP (Model Context Protocol) server extension so external tools/agents can drive dnSpy features via JSON‑RPC over SSE.

What’s included
- Visual Studio 2022 solution updates: The root solution has been updated for VS 2022 and project configuration mappings were refreshed for x86/x64/Any CPU targets.
- Minor code quality/perf tweaks: Small refactors (e.g., reverse iteration in GetLastDirectory) to simplify logic.
- MCP server extension: A new extension under `Extensions/Examples/Example1.Extension` adds an SSE‑based MCP server with tools, prompts, and resources:
  - JSON‑RPC methods: `initialize`, `tools/list`, `tools/call`, `prompts/list`, `prompts/get`, `resources/list`, and `resources/templates/list`.
  - SSE endpoint: `http://127.0.0.1:3003/sse` streams events; the first event exposes the POST endpoint for JSON‑RPC messages.
  - Tools surface dnSpy functionality (e.g., inspect and modify assemblies) to MCP clients.
  - Resources and templates follow the current MCP schema: concrete resources returned via `resources/list` (with `uri`), templates via `resources/templates/list` (with `uriTemplate`).

About the MCP server
- The extension is based on the AgentSmithers DnSpy MCP server concept and adapted to integrate with dnSpyEx and the current MCP spec.
- Protocol compatibility: The server speaks JSON‑RPC 2.0 over SSE and uses a recent MCP protocol version identifier in `initialize`.
- Copilot Chat integration: Add this server as an `sse` MCP endpoint in GitHub Copilot Chat (VS Code). The recent fixes ensure payloads match Copilot’s expected MCP shapes (e.g., `resources/list` returns only objects with `uri`).

Getting started
- Build: Open `dnSpy.sln` in Visual Studio 2022 and build. The extension project is included in the solution.
- Run dnSpyEx and enable/load the example extension (Example1.Extension). When active, it starts an MCP server at `127.0.0.1:3003`.
- Connect an MCP client:
  1) Open `http://127.0.0.1:3003/sse` (server‑sent events stream).
  2) Post JSON‑RPC requests to the endpoint announced by the first `event: endpoint` message (e.g., `/message?sessionId=...`).

Notes
- This fork is intentionally minimal and focused on MCP integration and build compatibility. For the broader feature set and documentation of dnSpyEx, see the upstream project.

License
- dnSpy/dnSpyEx are GPLv3. See `dnSpy/dnSpy/LicenseInfo/GPLv3.txt` and `dnSpy/dnSpy/LicenseInfo/CREDITS.txt` for details.
