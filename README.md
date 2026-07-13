# Conflict Guard

Conflict Guard makes a dumb sync pipe (OneDrive, Dropbox, Syncthing, SMB, iCloud, or
git) conflict-safe: concurrent edits to the same note merge via CRDT instead of
littering your vault with `note (conflict).md` copies or silently clobbering each
other, plus a best-effort "who was recently here" presence hint. Zero server —
nothing to host, no account, no daemon.

**This is a distribution-only repo.** It exists so [BRAT](https://github.com/TfTHacker/obsidian42-brat)
can install and update Conflict Guard from a private, invite-only source. Development,
issues, and source code live in the main project repo — see **Development** below.

## Supported substrates

| Substrate | Shape |
|---|---|
| OneDrive | shared cloud object store, own conflicted-copy naming |
| Dropbox | shared cloud object store (same shape as OneDrive) |
| Syncthing | peer-to-peer, own `.sync-conflict-*` + `.stversions/` bookkeeping |
| SMB / network shares | true last-writer-wins, no conflicted-copy net at all |
| iCloud Drive | shared cloud object store |
| git | content-addressed, merge-on-pull |

## Install via BRAT

This repo is **private**, so BRAT needs a GitHub **personal access token (PAT)** to
read it — your token, not shared with anyone, used only so your own copy of Obsidian
can fetch releases from this repo.

### 1. Create a personal access token

1. On GitHub: **Settings → Developer settings → Personal access tokens → Tokens
   (classic)** → **Generate new token (classic)**.
2. Give it a name like `brat-conflict-guard`, an expiration you're comfortable with,
   and check the **`repo`** scope (full control of private repositories — this is the
   scope BRAT's own token validator checks for). No other scopes are needed.
3. Generate it and copy the token (`ghp_...`) — GitHub only shows it once.

Fine-grained PATs work too if you prefer them: scope the token to this repository only,
with **Contents: Read-only** and **Metadata: Read-only** permissions.

### 2. Install BRAT (if you haven't already)

**Settings → Community plugins → Browse** → search **BRAT** → install and enable it.

### 3. Add Conflict Guard as a beta plugin

1. Open the command palette → **BRAT: Add a beta plugin for testing** (or **Settings →
   BRAT → Add beta plugin**).
2. In the **GitHub repository** field, enter:
   ```
   cybersader/obsidian-conflict-guard
   ```
3. In the **GitHub token** field of the same dialog, select/create a secret and paste
   the PAT from step 1 — BRAT stores it in Obsidian's secret storage, referenced by
   name, not in a plaintext settings file.
4. Click **Add Plugin**. BRAT will confirm once it has pulled the manifest and release
   assets.
5. Go to **Settings → Community plugins** and enable **Conflict Guard**.

BRAT will use the same stored token to check for and install updates going forward —
no need to re-enter it per release.

### Manual install (no BRAT)

Download `main.js` and `manifest.json` from the [latest release](https://github.com/cybersader/obsidian-conflict-guard/releases/latest)
(or the convenience `.zip`) and place them in
`<your-vault>/.obsidian/plugins/conflict-guard/`, then reload Obsidian and enable the
plugin in **Settings → Community plugins**.

## Documentation

The full picture — how it works, settings, `.crdt/` hygiene rules, honest limitations —
lives in the main project's docs site:

- [Conflict Guard vs. Obsidian Sync](https://docs.openclast.com/design/conflict-guard-vs-obsidian-sync/) — what it does and doesn't do, supported substrates
- [CRDT sync plugin design](https://docs.openclast.com/design/crdt-sync-plugin/) — how the merge engine works

## Development

Conflict Guard is built in the OpenClast monorepo (private), under `obsidian-plugin/`.
File issues and pull requests there — this repo is a publish target only, updated by an
automated release script, and does not accept direct commits or issues.

## License

AGPL-3.0-or-later. See [`LICENSE`](./LICENSE).
