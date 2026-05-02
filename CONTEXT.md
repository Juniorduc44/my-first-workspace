# Current Project

## What we are building
An open-source, cross-platform (Linux + Windows) Ollama chat application that:
- Installs in one command — a single downloaded script handles all dependencies, app setup, workspace scaffolding, and launching. The only thing a user must do manually is pull an Ollama model.
- Provides a web-first UI (single-page app) with optional native desktop wrappers (Electron or Tauri).
- Loads a workspace folder that defines persona, project context, references, templates, and local corpora to seed chat sessions (folder-driven ICM).
- Calls the local ollama CLI for inference, assembling deterministic prompts that respect token budgets and attach retrieved references from local files.
- Includes a simple retrieval layer (SQLite FTS for exact/local grounding; optional embeddings+hnswlib for semantic retrieval).
- Supports project templates, user snippets, session save/load, and provenance output (filename, line range, confidence).

## What good looks like
- Cross-platform starter repo with:
  - **One-click installers** — `install.sh` (Linux) and `install.ps1` (Windows) that are idempotent, safe to re-run for updates, and require zero prior developer knowledge to execute.
  - Clear folder spec and a sample workspace scaffolded automatically on first install.
  - Node backend that shells to ollama CLI safely on both OSes.
  - Web UI SPA with message history, file-search panel, and "Insert reference" workflow.
  - Prompt assembler that prepends curated context from the workspace and appends retrieval snippets prioritized by relevance and token cost.
  - Local retrieval using SQLite FTS v5 (or Windows-compatible alternative) with simple indexing scripts.
  - Tests for prompt assembly and CLI interaction (mocked).
  - Packaging scripts for Windows (MSI or NSIS) and Linux (AppImage or tarball) when using Electron; Tauri equivalents available.
- Reduced hallucinations by grounding answers with retrieved snippets and showing provenance metadata.

## One-click install requirements
The installer scripts (`install.sh` / `install.ps1`) must:
1. Detect and install Node.js ≥18 if missing (via `nvm` on Linux; `winget` or direct download on Windows).
2. Detect and install Ollama if missing (Ollama's own one-liner on Linux; `.exe` installer on Windows).
3. Download the latest app release from GitHub (tarball, no git required) or `git clone` if git is present.
4. Run `npm install` to set up dependencies.
5. Scaffold a default workspace folder with sensible starter files so the app works immediately on first launch.
6. Create a persistent launcher — a `.sh` or `.bat` file, desktop shortcut, or system service — so the user never needs a terminal again after install.
7. Open the browser automatically to `localhost:PORT` on first run.
8. Be idempotent — running the script again upgrades the app without breaking existing workspaces.
9. Pair with an `update.sh` / `update.ps1` script for future upgrades.

## First-run experience
- If Ollama is installed but has no models pulled, the UI must display a clear "Pull your first model" screen with copy-paste CLI instructions and links to the Ollama model library.
- The app must work with zero manual configuration on first launch — port, workspace path, and model fallback all have sensible built-in defaults.
- Mock mode activates automatically if Ollama is unreachable, with a visible banner explaining why and how to fix it.

## What to avoid
- Heavy remote dependencies or mandatory cloud services.
- Platform-specific binaries that are hard to install on Windows.
- Overly long context that exceeds model token limits without summarization or truncation strategy.
- Silent failures when ollama is missing — always show clear install/run guidance and fall back to a mock mode.
- Any install step that requires the user to open a terminal after the initial installer command.
