<!-- portfolio-context:start -->
# Portfolio Context

## What This Project Is

SnippetLibrary is a macOS menu bar app for managing reusable text snippets and inserting them into any focused app through a global hotkey. It combines SQLite FTS5 search, tags, recent snippets, import/export, and optional local Ollama embeddings so canned responses are searchable and pasteable without window switching or lasting clipboard pollution.

## Current State

The README describes the intended product surface and architecture: a floating search panel, FTS-backed snippet database, clipboard save/paste/restore injection, optional local semantic search, and macOS launch-at-login support.

## Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | SwiftUI |
| Storage | SQLite with FTS5 (via Swift package) |
| Highlighting | Highlightr |
| Embeddings | Ollama (optional, local) |

## How To Run

Build and run. Grant accessibility permissions when prompted (required for paste injection), then press `Cmd+Shift+Space` to open the search panel.

## Known Risks

- Paste injection requires accessibility permissions and should preserve the user's clipboard contents.
- Keep semantic search optional and local-only through Ollama.
- FTS5 ranking, tag filtering, and recent snippets are core workflow speed features; avoid replacing them with slower manual browsing.

## Next Recommended Move

Use this context plus the README and supporting docs to resume the next active task, then promote the repo beyond minimum-viable by capturing a dedicated handoff, roadmap, or discovery artifact.

<!-- portfolio-context:end -->
