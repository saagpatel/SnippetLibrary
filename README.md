# SnippetLibrary

[![Swift](https://img.shields.io/badge/Swift-f05138?style=flat-square&logo=swift)](#) [![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](#)

> Your entire canned-response library, one global hotkey away

SnippetLibrary is a macOS menu bar app for managing and inserting reusable text snippets. Press `Cmd+Shift+Space`, search your library, and selection simulates paste into the focused app, then restores the saved clipboard after 100 ms.

## Features

- **Global hotkey** — `Cmd+Shift+Space` opens a floating search panel from anywhere
- **FTS5 ranked search** — SQLite full-text search with title-match priority, FTS rank and multi-word query support
- **Tags and language filtering** — tags are supported by the repository and import/export and displayed in snippet details; the search panel filters by language, with no tag/category filter or tag editor
- **Paste injection** — saves the clipboard, simulates paste, then restores it after 100 ms; requires Accessibility permission and a target app that handles the paste in time
- **Recently used** — quick access to up to 5 snippets with nonzero usage, ordered by last edit time rather than insertion time
- **Ollama semantic search backend** — optional embedding-based similarity search exists in the repository but is not wired into the search panel; the endpoint defaults to localhost and is configurable
- **JSON import/export** — migrate your library between machines with merge or replace modes
- **Launch at login** — macOS 13+ `SMAppService` integration

## Quick Start

### Prerequisites
- Xcode 16+
- macOS 14.0+

### Installation
```bash
git clone https://github.com/saagpatel/SnippetLibrary
cd SnippetLibrary
open Package.swift
```

### Usage
Build and run. Grant Input Monitoring permission for the global hotkey and Accessibility permission for paste injection when prompted, then press `Cmd+Shift+Space` to open the search panel.

## Development verification

Run from the repository root on macOS 14+ with full Xcode 16+ (Swift 6) selected.
Check `xcode-select -p`: Command Line Tools alone are insufficient for the
SwiftUI `#Preview` macro plugin used by this app. If compilation reports a
missing `PreviewsMacros` plugin, use an installed full Xcode toolchain (for
example, set `DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer` for
that shell) before rerunning; do not remove preview code to validate docs.
Swift Package Manager resolves GRDB and Highlightr from `Package.resolved`;
initial dependency checkout needs network access. Review any unexpected lockfile
change rather than treating an upgrade as verification.

```bash
swift build
swift test --filter DatabaseTests   # focused in-memory database coverage
swift test --filter SearchTests     # focused FTS/search coverage
swift test                         # full suite: use a disposable macOS test user
```

`make build` and `make test` wrap the first and last commands. No separate
lint, formatter or typecheck command is configured; the build compiles Swift.
Database/search/import tests use `AppDatabase.makeEmpty()` (in-memory SQLite),
and paste tests inject mock clipboard/event clients. Ollama request tests use
invalid endpoints; the configuration test uses localhost without making a request.
Ollama and import tests call `OllamaService.configure()`, which writes
endpoint/model/enabled keys through `UserDefaults.standard`. Saving/restoring
those keys still changes persisted preferences during the tests. Database/Search
inserts also launch embedding tasks that can send requests if saved Ollama
preferences enable them. Run all test lanes in a disposable macOS test user
with Ollama disabled, as CI uses an ephemeral runner. A running
Ollama server is not a prerequisite for unit tests. Tests do not prove real
paste permissions, hotkeys or successful live embeddings.

For changed native UI, search, import/export or paste behavior, additionally
check the relevant flow with synthetic snippets in a separate macOS test user.
Normal app launch (`swift run` or Xcode Run) uses that user's
`~/Library/Application Support/SnippetLibrary/snippets.sqlite`; it can prompt
for Accessibility and affect the clipboard/focused app. Use a scratch text
editor for paste checks and keep launch-at-login disabled for the test.
Do not use a personal snippet library for verification. This is a native app,
so browser checks are not applicable. Local build/test success is separate
from packaging, signing and distribution, which have no configured release gate.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | SwiftUI |
| Storage | SQLite with FTS5 (via Swift package) |
| Highlighting | Highlightr |
| Embeddings | Ollama (optional, localhost by default; configurable endpoint) |

## Architecture

The menu bar app maintains a persistent SQLite database with an FTS5 virtual table for full-text search. On hotkey, a floating NSPanel appears with a search field wired to the FTS5 query. Selection triggers a clipboard save → programmatic paste → clipboard restore sequence using `CGEvent` synthesis. The optional repository semantic search method embeds the query via the configured Ollama endpoint and performs a cosine similarity scan over pre-computed snippet embeddings stored in the `snippet.embedding` BLOB column; the search panel calls only FTS search.

## License

MIT
