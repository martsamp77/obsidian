# Obsidian vault (forge42)

Personal Obsidian vault, version-controlled with Git. The vault root is the [`forge42/`](forge42/) folder; Obsidian Sync keeps it available on mobile and other devices.

## Table of contents

- [Overview](#overview)
- [Opening the vault](#opening-the-vault)
- [Sync vs Git](#sync-vs-git)
- [Repository layout](#repository-layout)
- [Playbook in this repo](#playbook-in-this-repo)
- [Git hygiene](#git-hygiene)

## Overview

Notes live as local Markdown files under `forge42/`. You edit on a desktop (or any machine where you open this folder as a vault). **Obsidian Sync** replicates the vault to your phone and other logged-in devices so the same notes are available everywhere Obsidian runs. **Git** (this repository) is the version history and offsite backup: diffs, rollbacks, and a copy independent of Obsidian’s servers. Sync and Git solve different problems; both can be used together as long as the vault folder is not also synced by a generic cloud folder (OneDrive, Dropbox, iCloud) competing with Obsidian Sync—see [Obsidian Master Guide 2026](forge42/Obsidian%20Master%20Guide%202026.md) for why.

## Opening the vault

1. Clone this repository (or use your existing checkout).
2. In Obsidian, choose **Open folder as vault** and select the **`forge42`** directory inside the repo (e.g. `...\obsidian\forge42` on your machine).

## Sync vs Git

- **Obsidian Sync** — Real-time sync between devices running Obsidian under your account; handles the `.obsidian` config in a way intended for the app. Use it when you want the vault on mobile.
- **Git** — Commits, branches, and remotes for history and backup. It is not a live multi-device editor sync layer.

Do not point OneDrive, Dropbox, or iCloud at the same vault folder while using Obsidian Sync; two sync layers on the same tree cause conflicts and corrupted settings. Keep the vault in a normal local path and let Obsidian Sync own cross-device sync for Obsidian.

## Repository layout

| Path | Purpose |
|------|---------|
| [`forge42/`](forge42/) | Vault root: notes, attachments, and [`.obsidian/`](forge42/.obsidian/) (app settings). |
| [`forge42/Home/`](forge42/Home/), [`forge42/Tech/`](forge42/Tech/) | Topic folders for notes. |
| [`forge42/_me/`](forge42/_me/) | Personal sub-area (e.g. health tracking). Has its own [README](forge42/_me/README.md). |

Other Markdown files may sit at the top of `forge42/` (tasks, scratch, migration plans, etc.).

## Playbook in this repo

Long-form setup, settings, folder philosophy, plugins, multi-vault ideas, and step-by-step Sync notes are in **[Obsidian Master Guide 2026](forge42/Obsidian%20Master%20Guide%202026.md)**. Use that document as the detailed playbook; this README stays a short map of the repo.

## Git hygiene

[`.gitignore`](.gitignore) excludes noisy or machine-specific files under the vault, including:

- `forge42/.obsidian/workspace.json` and `workspace-mobile.json`
- `forge42/.obsidian/plugins/recent-files-obsidian/data.json`
- `forge42/.trash/`

Most of `.obsidian/` can still be committed so plugin and editor settings are versioned; workspace JSON is omitted to avoid constant meaningless diffs. Align further ignores with the recommendations in the master guide if you add plugins that write volatile data.
