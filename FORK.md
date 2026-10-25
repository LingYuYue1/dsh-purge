# Personal fork: dsh-purge

This repository is a personal fork of
[YuJunZhiXue/dsh-purge](https://github.com/YuJunZhiXue/dsh-purge), maintained by
[LingYuYue1](https://github.com/LingYuYue1) for self-use.

原项目：<https://github.com/YuJunZhiXue/dsh-purge>（作者：小杨 / 叫我小杨同学，MIT）。
本仓库保留原项目的 LICENSE、README 与署名要求；下面的改动仅用于让插件继续适配新版
DeepSeek Harness，不代表原作者。

Upstream paused development on 2026-10-12 ("docs: 暂停更新说明"). This fork keeps the
plugin usable on newer DSH releases and is not affiliated with the original author.

## Changes

### 1.1.72 — DSH 0.2.1-rc.2 compatibility (2026-10-18)

- `lib/index.js`: register the `cwd` prompt variable from `apply()`.
  DSH 0.2.1-rc.2 only registers `provider` and `model`; the plugin's bundle patch
  still sets `personaSuffix: "Your working directory is {{cwd}}."`, which made
  prompt assembly throw:

  ```text
  unknown prompt variable "{{cwd}}" in section "deployment:persona-suffix"
  ```

  The registered value follows the session's current `workingDirectory`, falling
  back to `session.header.cwd`, then `process.cwd()`.
- `lib/update.js`: built-in updater now tracks this fork instead of upstream.
- `package.json`: `version` bumped to `1.1.72`; repository/homepage/issues and
  `dsh.install` URLs point at this fork.
- `client.js` / `lib/redteam/ui.js`: repository links point at this fork.

No upstream feature changes were made in this release.

## Install from this fork

```bash
# Web / headless profile
dsh plugin --profile web add https://codeload.github.com/LingYuYue1/dsh-purge/tar.gz/refs/heads/master
```

For the Desktop profile, install/update through the in-app plugin market and use
the same tarball URL.

## Updating this fork

```bash
git fetch upstream
git merge upstream/master      # or rebase, if you prefer a linear history
# re-apply the compatibility patch if lib/index.js changed
```

The compatibility change is intentionally small (22 added lines). If upstream
ever fixes `{{cwd}}` itself, this patch can be dropped.

## Branch `fuxuan` — workspace-neutral inject version (2026-10-25)

`prompt-inject.fuxuan.md` is this fork's replacement for the `~/.dsh/prompt-inject.md`
override. Every execution-strength section is kept verbatim (Sage Mode, HARD OUTPUT
RULES, no-refusal clauses, task templates, CTF contracts). Only the persona layer changes:

- owner name unified to 牢凌 across the file (15 occurrences, case-sensitive replacement).
- the inject no longer self-names as "Little Code Sauce"; identity is owned by each
  agent preset and its 运行卡, so the five workspace members do not inherit each
  other's names.
- the girlfriend / novelist / "two years together" block is replaced with a colleague
  relation that keeps the same insistence on delivering.
- `keygen` template lines are untouched — a case-insensitive global replace would have
  corrupted them (`keygen` contains a lowercase `yg`).

Install: copy it over `~/.dsh/prompt-inject.md`, restart the app, then refresh
`override-state.json`'s `hash` to the new file's SHA-256. Changes only take effect in
sessions created after the restart.
