<!-- portfolio-context:start -->
# Portfolio Context

## What This Project Is

SnippetLibrary is a macOS menu bar app for managing reusable text snippets and simulating paste into the focused app through a global hotkey. It combines SQLite FTS5 search, stored tags, previously used snippets, import/export, and optional Ollama embeddings (localhost by default, with a configurable endpoint). Paste saves the clipboard and restores it after 100 ms; the target app must handle the paste in time.

## Current State

The README describes the product surface and architecture: a floating search panel, FTS-backed snippet database, clipboard save/paste/restore injection, an optional repository semantic search method not wired into the search panel, and macOS launch-at-login support.

## Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | SwiftUI |
| Storage | SQLite with FTS5 (via Swift package) |
| Highlighting | Highlightr |
| Embeddings | Ollama (optional, localhost by default; configurable endpoint) |

## How To Run

Build and run. Grant Input Monitoring permission for the global hotkey and Accessibility permission for paste injection when prompted. After granting Input Monitoring, quit and relaunch the app, then press `Cmd+Shift+Space` to open the search panel.

## Known Risks

- Paste injection requires accessibility permissions and should preserve the user's clipboard contents.
- Keep semantic search optional and local-only through Ollama.
- FTS5 ranking, language filtering (tag filtering intended; not yet in the search panel), and previously used snippets are core workflow speed features; avoid replacing them with slower manual browsing.

## Next Recommended Move

Use this context plus the README and supporting docs to resume the next active task, then promote the repo beyond minimum-viable by capturing a dedicated handoff, roadmap, or discovery artifact.

<!-- portfolio-context:end -->
