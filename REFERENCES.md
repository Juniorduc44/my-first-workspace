# References

## Examples of good work
- Local-first CLI wrappers with web UIs (pattern: backend shells to local binary, SPA talks to backend).
- Folder-driven context approaches ("context-as-code") that use small files to define identity, scope, and references.
- Simple provenance citation in model outputs (filename + snippet + confidence).

## Relevant tools & links
- Ollama CLI (local model serving) — use local installation; ensure cross-platform guidance in README.
- Node.js (>=18) — backend runtime for shelling to ollama and running SQLite.
- Electron (https://www.electronjs.org/) — native desktop option (cross-platform).
- Tauri (https://tauri.app/) — lightweight native desktop option (cross-platform; Rust tooling).
- SQLite with FTS5 — local full-text search for grounding (works on Windows and Linux).
- SQLite + embeddings: use python script or Node bindings to build optional vector index (Faiss/hnswlib/annoy) — Faiss less friendly on Windows; prefer hnswlib or a small cloudless alternative.
- Embedding providers: use local embedding models where possible; otherwise make embeddings optional.
- Cross-platform packagers: NSIS (Windows), AppImage / deb / rpm / AppImage (Linux) or Tauri build pipeline.
- Web UI frameworks: React / Vite / Svelte (choose minimal + fast).
- Prompt-engineering guides and token-budget strategies (summarize/chain-of-thought suppression patterns).

## Notes
- Always have token count stats in and out and total for users to see for references of use per llm or in general.
- Provide example install notes for both OSes in README; include automated checks for ollama binary and instructions to install it manually if missing.
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