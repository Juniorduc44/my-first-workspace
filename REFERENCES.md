# References

## Examples of good work
- Local-first CLI wrappers with web UIs (pattern: backend shells to local binary, SPA talks to backend).
- Folder-driven context approaches ("context-as-code") that use small files to define identity, scope, and references.
- Simple provenance citation in model outputs (filename + snippet + confidence).
- One-command installers (pattern: detect → install deps → download app → scaffold → launch → open browser).

## Relevant tools & links
- Ollama CLI (local model serving) — use local installation; ensure cross-platform guidance in README.
  - Linux one-liner: `curl -fsSL https://ollama.com/install.sh | sh`
  - Windows: download and run the `.exe` from https://ollama.com/download/windows
- Node.js (>=18) — backend runtime for shelling to ollama and running SQLite.
  - Linux: install via `nvm` (https://github.com/nvm-sh/nvm) for version flexibility.
  - Windows: install via `winget install OpenJS.NodeJS.LTS` or direct download from https://nodejs.org.
- nvm (https://github.com/nvm-sh/nvm) — Node version manager for Linux/macOS install automation.
- nvm-windows (https://github.com/coreybutler/nvm-windows) — Windows equivalent for automated Node install.
- winget — built-in Windows package manager (available on Windows 10 1709+); use for Node and other deps.
- Electron (https://www.electronjs.org/) — native desktop option (cross-platform).
- Tauri (https://tauri.app/) — lightweight native desktop option (cross-platform; Rust tooling).
- SQLite with FTS5 — local full-text search for grounding (works on Windows and Linux).
- SQLite + embeddings: use Node bindings to build optional vector index (hnswlib preferred over Faiss for Windows compatibility).
- Embedding providers: use local embedding models where possible; otherwise make embeddings optional.
- Cross-platform packagers: NSIS (Windows), AppImage / deb / rpm (Linux) or Tauri build pipeline.
- Web UI frameworks: React / Vite / Svelte (choose minimal + fast).
- Prompt-engineering guides and token-budget strategies (summarize/chain-of-thought suppression patterns).
- `open` / `start` / `xdg-open` — cross-platform browser-open commands usable from Node.js `child_process` to auto-launch the UI after install.

## Installer script spec
Two scripts, one per platform:
- `install.sh` — Bash, targets Linux (and macOS as a bonus).
  - Entry point: `curl -fsSL https://your-repo/install.sh | bash`
- `install.ps1` — PowerShell, targets Windows 10/11.
  - Entry point: `irm https://your-repo/install.ps1 | iex`

Both scripts follow the same step order:
1. Check OS and architecture.
2. Install Node.js ≥18 if not present.
3. Install Ollama if not present.
4. Download latest app release (GitHub releases tarball; fallback to `git clone`).
5. `npm install --omit=dev` inside app directory.
6. Scaffold default workspace (`~/ollama-chat-workspace/`) if it doesn't exist.
7. Write a launcher script (`~/bin/ollama-chat.sh` or `%USERPROFILE%\bin\ollama-chat.bat`).
8. Optionally create a desktop shortcut.
9. Start the server and open `http://localhost:3000` in the default browser.

Idempotency rule: if a step's target already exists and is valid, skip it with a status message. Never overwrite existing workspace files.

Companion scripts:
- `update.sh` / `update.ps1` — pull latest release, re-run `npm install`, restart server.
- `uninstall.sh` / `uninstall.ps1` — remove app files and launchers; leave workspace folder in place by default (prompt user).

## Notes
- Always have token count stats in and out and total for users to see for references of use per llm or in general.
- Workspace folder spec (minimum files):
  - CLAUDE.md (identity + rules)
  - CONTEXT.md (project context + goals)
  - REFERENCES.md (links, notes, examples)
  - /snippets/*.md (reusable text snippets)
  - /corpora/*.txt (documents to index)
  - /templates/*.prompt (message templates)
  - workspace.json (optional metadata: token budget, preferred model, retrieval config)
- Grounding strategy: index corpora with SQLite FTS, on query run a top-K retrieval, then include the top N snippets with file:line metadata in the prompt; if token budget might overflow, include short summaries instead of full snippets.
- For Windows compatibility: avoid native OS-specific node modules that require compiled binaries (or provide prebuilt binaries). Favor pure JS or cross-compiled libs.
- For packaging: test both electron-builder (Windows NSIS) and Tauri (for smaller binaries) and include a web-only distribution for edge cases.
- First-run UX: if `ollama list` returns no models, render a "Pull your first model" screen in the UI with copy-paste instructions (e.g., `ollama pull llama3`) and a link to https://ollama.com/library.
- The app's default port (3000) should be configurable via `workspace.json` or an env var, with automatic port-conflict detection that increments to the next available port.
