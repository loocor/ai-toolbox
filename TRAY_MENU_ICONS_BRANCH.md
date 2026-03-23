# Tray menu icons branch workflow

This fork keeps **system tray menu CLI icons** (and related assets) on branch `feature/tray-menu-icons`. Branch `main` tracks [upstream `coulsontl/ai-toolbox`](https://github.com/coulsontl/ai-toolbox) so routine merges stay aligned with the original project.

Upstream reverted the tray icon work (see PR #100); this branch preserves it for local builds and experiments. **Windows tray appearance** is left for a later pass.

## One-time setup

Add upstream (skip if already configured):

```bash
git remote add upstream https://github.com/coulsontl/ai-toolbox.git
```

## Keep `main` in sync with upstream

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

Use `git pull --ff-only` instead of `merge` if you prefer fast-forward only.

## Build or run **with** tray menu icons

Merge the feature branch into your current line (often `main` after syncing):

```bash
git checkout main
git merge feature/tray-menu-icons
```

Resolve conflicts if any (usually `tauri/src/tray.rs`, `tauri/src/lib.rs`, and tray icon assets). Then build as usual, e.g. `pnpm tauri dev`.

To publish only from a throwaway branch instead of `main`:

```bash
git checkout -b build/with-tray-icons main
git merge feature/tray-menu-icons
```

## Refresh **feature/tray-menu-icons** on top of upstream

Do this periodically so the feature branch does not drift too far from upstream (smaller conflicts over time):

```bash
git checkout feature/tray-menu-icons
git fetch upstream
git merge upstream/main
# fix conflicts, test, then:
git push origin feature/tray-menu-icons
```

Alternatively merge updated `origin/main` if it already matches upstream:

```bash
git checkout feature/tray-menu-icons
git fetch origin
git merge origin/main
git push origin feature/tray-menu-icons
```

## What this branch typically includes

- `tauri/src/tray_cli_icons.rs` and `pub mod tray_cli_icons` in `tauri/src/lib.rs`
- Tray menu construction in `tauri/src/tray.rs` wired to those icons
- PNGs under `tauri/icons/tray/cli/` (and related tray assets)

If any of these are dropped during a merge, the feature will not build or will silently lose icons.

## Optional safety tag

After a clean merge or release:

```bash
git tag tray-menu-icons-YYYY-MM-DD
git push origin tray-menu-icons-YYYY-MM-DD
```

## Related upstream context

- [PR #99](https://github.com/coulsontl/ai-toolbox/pull/99) — original tray icon contribution (merged then reverted)
- [PR #100](https://github.com/coulsontl/ai-toolbox/pull/100) — revert of that feature on upstream `main`
