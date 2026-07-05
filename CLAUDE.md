# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This is a **Doom Emacs private configuration** directory (`~/.config/doom`), on macOS.

## Editing model: config.org is the source of truth

The `literate` Doom module (see `init.el` `:config`) plus `org-auto-tangle` means **`config.org` is tangled to `config.el`**. Never edit `config.el`, `config.el.bak`, or `config.html` — they are generated/backup/export artifacts and will be overwritten.

- Edit configuration in `config.org`. `org-auto-tangle-default` is `t`, so saving the buffer regenerates `config.el`; `doom sync` also tangles it.
- Structure: five top-level `*` groups — **CORE & DEFAULTS**, **UI & APPEARANCE**, **CODING / LSP**, **ORG**, **APPS & TOOLS** — each with per-topic `**` subsections (e.g. `** EGLOT`) containing `#+begin_src emacs-lisp` blocks. Add new config as a `**` under the fitting group, not a new top-level `*`.
- Prefer Doom idioms already used throughout: `use-package!`, `map!` (with `:leader` and `:prefix`), `setq`, `add-hook!`, `after!`.

## The three entry files

- `init.el` — module manifest (`doom! ...`). Enables/disables Doom modules and their flags. Editing this requires `doom sync`.
- `packages.el` — third-party `package!` declarations (and `:recipe` for non-MELPA sources). Editing this requires `doom sync`.
- `config.org` → `config.el` — all actual configuration.

`early-init.el` sets `LIBRARY_PATH` from Homebrew GCC/libgccjit so native compilation works on macOS; it runs before Doom loads.

## Common commands

```sh
doom sync        # after editing init.el or packages.el (installs/removes pkgs, tangles config.org)
doom sync -u     # sync and update package versions
doom upgrade     # update Doom itself + packages
doom doctor      # diagnose config/env problems
doom build       # byte-compile packages
```

Inside Emacs: `SPC h r r` (`doom/reload`) reloads config without restarting; module changes still need a full `doom sync` + restart.

There is no test suite. Validate changes by tangling, `doom sync` if modules/packages changed, then reloading or restarting Emacs and confirming no errors in `*Messages*` / `doom doctor`.

## Notable choices (context before you change things)

- **LSP via EGLOT**, not lsp-mode (`(lsp +eglot)`). Debugging via **DAPE**, not dap-mode.
- **Completion:** ivy (`+prescient +icons`, posframe), not vertico/helm. Evil everywhere.
- **Primary languages:** Go, Python (both `+tree-sitter`), plus clojure, js, sh, sql, web, yaml, json.
- **`claude-code-ide`** (the `* CLAUDE CODE IDE` section) is heavily tuned: `ghostel` terminal backend, right side-window, ediff-based diffs, MCP server exposing xref/tree-sitter/imenu/project, and a mouse-wheel workaround for `no-flicker` fullscreen rendering. The comments there explain the *why* of each anti-flicker / scroll setting — read them before touching this section.
- `start.org` is a custom dashboard/start-page (keybinding cheatsheet). `modules/` is currently empty.
