# SnippetLibrary — Portfolio Disposition

**Status:** Active — working Swift macOS menu bar app on `origin/main`,
no release-readiness work yet. Disposition is **not** Release Frozen;
the gate isn't Apple signing, it's "decide whether to package this
for distribution at all." Same shape as Conductor's disposition.

> Disposition uses strict `origin/main` verification.

---

## Verification posture

This repo has clean account-migration state — no `legacy-origin`
remote. The `legacy-origin` trap from FreeLanceInvoice and
PersonalKBDrafter doesn't apply here.

Specifically verified on `origin/main`:
- Tip: `4e1bfba` docs(context): recover portfolio context (#6)
- Last substantive feature commit: `9dcb1fd feat: add GitHub Actions
  CI workflow` (CI plumbing, not product feature)
- Source tree: `SnippetLibrary/`, `Tests/`, `scripts/`,
  `Package.swift`, `Package.resolved` — 28 source files
- Default branch: `main`
- No `docs/` directory other than this file

---

## Current state in one paragraph

SnippetLibrary is a macOS menu bar app that surfaces a global hotkey
(`Cmd+Shift+Space`) snippet picker for instantly inserting reusable
text into any focused app. Uses SQLite FTS5 for ranked search,
optional local Ollama embeddings for semantic similarity, paste
injection via clipboard save/restore + simulated keystroke,
SMAppService for launch-at-login. Swift Package Manager
(`Package.swift`) drives the build; tests under `Tests/`; CI on
GitHub Actions. The product surface is real and functional, but no
release-readiness packet has been written — no signing checklist,
no notarization plan, no release runbook.

For full detail see `README.md`.

---

## Why "Active" instead of Release Frozen

The signing-frozen cluster (DesktopPEt / ContentEngine / AIGCCore /
Relay / FreeLanceInvoice / Nexus / DeepTank — 7 repos) has one
common property: each has a release-readiness artifact (runbook,
hard gates, signing checklist). SnippetLibrary does not. The
product surface is real but the path from "works locally" to
"distributed binary someone else uses" hasn't been scoped.

Same pattern as Conductor: real product, no release path defined,
operator must pick what to do next.

---

## Possible next moves (operator choice)

### Option 1 — Package for distribution

Required scope:

1. Write `docs/RELEASE-READINESS.md` (akin to the signing-cluster
   members).
2. Wire `xcodebuild`-side signing + notarization via CI, or use
   Swift Package's archive workflow.
3. Cut a v0.1.0 release as a signed `.app` bundle (or homebrew
   tap; SnippetLibrary's "menu bar utility" shape is a natural fit
   for `brew install --cask`).
4. Then transition disposition to `Release Frozen` and join the
   signing cluster (which becomes 8 repos).

Estimated effort: ~4 hours for signing setup + first notarization +
homebrew tap submission. Slightly easier than the other
signing-cluster members because Swift Package builds are simpler
than Tauri / Electron / Xcode-managed apps.

### Option 2 — Open-source as a developer tool, no packaged binary

Keep `main` public, document local-build workflow in README ("clone,
run `swift build -c release`, drop the binary in your `~/bin`"), do
nothing about signing. Users who want it build it themselves. Same
posture as Option 2 for Conductor.

Estimated effort: ~30 minutes — polish the README "How to install"
section.

### Option 3 — Mark as personal-use tool

Decide the audience is just the author. Move to `Cold Storage` with
explicit disposition "personal-use, no distribution path." Keep the
repo public for transparency, but stop treating it as something with
a release packet.

Estimated effort: ~15 minutes.

---

## Recommendation (informational, not committed)

**Option 1 is probably the right call** for SnippetLibrary
specifically:

- The product (global-hotkey snippet manager) has obvious audience
  appeal — same category as TextExpander / aText / Espanso, all of
  which have paying users.
- The "local Ollama embeddings for semantic similarity" feature is a
  real differentiator and signals the maker cares about local-first
  AI integration.
- The "menu bar utility" shape is a natural homebrew cask candidate
  — that distribution path is well-trodden and doesn't require a
  custom downloads page.

But this is operator-judgment. Option 2 is fine if the operator
doesn't want to operate a release pipeline yet.

---

## Portfolio operating system instructions

| Aspect | Posture |
|---|---|
| Portfolio status | `Active` |
| Next packet shape | "Decide between Option 1 / 2 / 3 above" |
| Review cadence | Resume normal cadence — this row needs decision-time |
| Resurface conditions | Once the operator picks an option, surface a packet for the work each option implies |
| Do **not** auto-add to signing cluster | The cluster is for repos that already have release-readiness docs. SnippetLibrary doesn't — adding it would bring an unrelated decision into a signing session. Same exclusion logic as Conductor. |

---

## Reactivation procedure (for the next code session)

1. Verify `git branch -vv` shows `main` tracking `origin/main`.
   This repo has no `legacy-origin` remote so the trap isn't here,
   but verify anyway as a habit.
2. Delete stale `codex/*` branches that pre-date the CI workflow
   commit.
3. Re-run `swift build` and `swift test` to confirm the toolchain
   still works after the freeze.
4. If picking Option 1, write the release-readiness doc first.
   Don't start signing work without that doc in place.

---

## Last known reference

| Field | Value |
|---|---|
| `origin/main` tip | `4e1bfba` docs(context): recover portfolio context (#6) |
| Last substantive commit | `9dcb1fd` feat: add GitHub Actions CI workflow |
| Default branch | `main` |
| Build system | Swift Package Manager (`Package.swift` + `Package.resolved`) |
| Tests present | Yes (`Tests/` directory) |
| CI present | Yes (GitHub Actions workflow added recently) |
| Release readiness doc | **None** — gate before joining the signing cluster |
| Migration state | Clean (no `legacy-origin` remote) |
| Natural distribution path | Homebrew cask (menu bar utility convention) |
