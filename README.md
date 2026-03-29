# SnippetLibrary

[![Swift](https://img.shields.io/badge/Swift-f05138?style=flat-square&logo=swift)](#) [![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](#)

> Your entire canned-response library, one global hotkey away

SnippetLibrary is a macOS menu bar app for managing and instantly inserting reusable text snippets. Press `Cmd+Shift+Space`, search your library, and the selected snippet pastes directly into whatever app is in focus — no clipboard pollution, no window switching.

## Features

- **Global hotkey** — `Cmd+Shift+Space` opens a floating search panel from anywhere
- **FTS5 ranked search** — SQLite full-text search with title/content weighting and multi-word query support
- **Tag management** — organize snippets with tags and filter by language or category
- **Paste injection** — inserts via clipboard save/restore + simulated paste, so the snippet lands in any app
- **Recently used** — quick access to the last 10 inserted snippets
- **Local semantic search** — optional Ollama embedding-based similarity search for fuzzy intent matching
- **JSON import/export** — migrate your library between machines with merge or replace modes
- **Launch at login** — macOS 13+ `SMAppService` integration

## Quick Start

### Prerequisites
- Xcode 16+
- macOS 13.0+

### Installation
```bash
git clone https://github.com/saagpatel/SnippetLibrary
open SnippetLibrary.xcodeproj
```

### Usage
Build and run. Grant accessibility permissions when prompted (required for paste injection), then press `Cmd+Shift+Space` to open the search panel.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | SwiftUI |
| Storage | SQLite with FTS5 (via Swift package) |
| Highlighting | Highlightr |
| Embeddings | Ollama (optional, local) |

## Architecture

The menu bar app maintains a persistent SQLite database with an FTS5 virtual table for full-text search. On hotkey, a floating NSPanel appears with a search field wired to the FTS5 query. Selection triggers a clipboard save → programmatic paste → clipboard restore sequence using `CGEvent` synthesis. The optional semantic search path embeds the query via a local Ollama model and performs a cosine similarity scan over pre-computed snippet embeddings stored in a separate SQLite table.

## License

MIT