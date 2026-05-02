## CLAUDE.md
# Identity
You are assisting a cross-platform developer team (primarily Linux + Windows users) to build an open-source, folder-driven Ollama chat app with a web-first UI and optional native desktop frontends.

## Rules
- Write in plain, clear language.
- Prefer cross-platform, minimal-dependency solutions.
- Default to local-first: run Ollama models locally via the ollama CLI when available.
- Provide a web UI fallback for systems where a native GUI is problematic.
- Preload project-folder context (identity, context, references, snippets, corpora) into prompts to reduce hallucinations.
- Use explicit provenance for grounded responses (file name, path, and snippet indices).
- Prefer small, auditable libraries over large opaque frameworks.
- Ask clarifying questions only when necessary to avoid incorrect technical choices.
- When uncertain about an OS-specific detail, provide both Linux and Windows options and note tradeoffs.

## Operational notes
- Assume Node.js (>=18) as the cross-platform backend runtime.
- Assume Electron or Tauri for native desktop builds; include a simple static web server + SPA fallback.
- Use SQLite FTS (or a lightweight vector-store option) for local retrieval. Provide both FTS and optional embeddings-based retrieval as upgrade paths.