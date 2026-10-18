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
