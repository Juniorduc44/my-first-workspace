# Current Project

## What we are building
An open-source, cross-platform (Linux + Windows) Ollama chat application that:
- Provides a web-first UI (single-page app) with optional native desktop wrappers (Electron or Tauri).
- Loads a workspace folder that defines persona, project context, references, templates, and local corpora to seed chat sessions (folder-driven ICM).
- Calls the local ollama CLI for inference, assembling deterministic prompts that respect token budgets and attach retrieved references from local files.
- Includes a simple retrieval layer (SQLite FTS for exact/local grounding; optional embeddings+annoy/faiss/hnswlib for semantic retrieval).
- Supports project templates, user snippets, session save/load, and provenance output (filename, line range, confidence).

## What good looks like
- Cross-platform starter repo with:
  - Clear folder spec and a sample workspace.
  - Node backend that shells to ollama CLI safely on both OSes.
  - Web UI SPA with message history, file-search panel, and "Insert reference" workflow.
  - Prompt assembler that prepends curated context from the workspace and appends retrieval snippets prioritized by relevance and token cost.
  - Local retrieval using SQLite FTS v5 (or Windows-compatible alternative) with simple indexing scripts.
  - Tests for prompt assembly and CLI interaction (mocked).
  - Packaging scripts for Windows (MSI or NSIS) and Linux (AppImage or tarball) when using Electron; Tauri equivalents available.
- Reduced hallucinations by grounding answers with retrieved snippets and showing provenance metadata.

## What to avoid
- Heavy remote dependencies or mandatory cloud services.
- Platform-specific binaries that are hard to install on Windows.
- Overly long context that exceeds model token limits without summarization or truncation strategy.
- Silent failures when ollama is missing—always show clear install/run guidance and fall back to a mock mode.