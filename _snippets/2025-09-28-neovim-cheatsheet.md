---
title: "Python Neovim cheathseet"
date: 2025-09-28
tags: [python, neovim, tips]
permalink: /snippets/neovim-cheatsheet/
---

This is the single, comprehensive guide for mastering Neovim itself and the Python‑focused setup in your `init.lua`. It combines fundamentals, advanced workflows, and every keymap from your configuration with clear explanations.

Legend: `<leader>` is space. Modes: `n` = normal, `t` = terminal, `x` = visual.


## 1) Neovim Fundamentals

- Movement
  - Words: `w`/`b`/`e`, big words: `W`/`B`/`E`
  - Lines: `0` or `^` to start, `$` to end
  - File: `gg` top, `G` bottom; `H/M/L` to top/middle/bottom of screen
  - Scroll: `<C-u>` half up, `<C-d>` half down, `<C-f>/<C-b>` page
  - Match pairs: `%` jumps between matching `()[]{}`
- Search
  - `/pattern` forward, `?pattern` backward; navigate `n`/`N`
  - Clear highlights: `<Esc>` (mapped)
- Jumps and marks
  - Jumplist back/forward: `<C-o>` / `<C-i>`
  - Alternate buffer (last buffer in this window): `<C-^>` or `:b#`
  - Marks: `ma` to set, `'a`/`` `a`` to jump. Special: ```` (last jump), `'.` (last edit)
- Visual modes and text objects
  - Visual: `v` charwise, `V` linewise, `<C-v>` blockwise
  - Text objects: words `iw/aw`, quotes/parens/braces `i'`/`a'`, `i"/a"`, `i(`/`a(`, `i{`/`a{`, paragraph `ip/ap`
  - Combine with operators: `d`, `c`, `y`, `v` (e.g., `ci"`, `da)`, `viw`)
- Folds (compact large code blocks)
  - Toggle: `za`, open/close: `zo`/`zc`, all: `zR`/`zM`, more/less: `zr`/`zm`
- Files and explorer
  - Edit: `:e path` (tab completes)
  - Built‑in explorer: `:Ex`/`:Explore` (`<CR>` open, `-` up, `v`/`s` split)
- Built‑in terminal
  - Open: `:terminal`; exit to Normal mode: `<Esc><Esc>` (mapped)
- Save/quit
  - `:w` write, `:wa` write all; `:q` quit window, `:qa` quit all, `:wqa` write+quit all


## 2) Windows, Buffers, Tabs

Buffers are open files, windows are views (splits), tabs are layouts containing windows.

- Window focus
  - `<C-h>`, `<C-j>`, `<C-k>`, `<C-l>` — move focus left/down/up/right (mapped)
- Splits
  - `:split` / `:vsplit` (or `<leader>ws` / `<leader>wv`) to open horizontal/vertical splits
  - Equalize sizes: `<C-w>=` (mapped to `<leader>w=`)
  - Close current window: `:q` or `<leader>wc`; keep only this one: `:only` or `<leader>wo`
- Tabs (use as “workspaces/layouts”)
  - New tab: `:tabnew` or `<leader>ta`; close: `:tabclose` or `<leader>tc`; only: `<leader>to`
  - Next/prev: `gt` / `gT`, jump to Nth: `{N}gt` or `<leader>t1..t9`
- Buffers
  - List: `:ls`; next/prev: `<leader>bn` / `<leader>bp`; delete: `<leader>bd`
  - Alternate buffer (last in this window): `<leader>a` (`:b#`) — great for quick toggles
  - Keep only current buffer: `<leader>bo` (closes all others without destroying the window)

Agile split + buffer workflow
- Open file in right split straight from Telescope: `<leader>sf` → pick → `<C-v>`
- Keep “home” file on the left; explore on the right; flip last file in any window with `<leader>a`
- Create split first (`:vsplit`), then place an existing buffer via `:ls` → `:buffer {nr}` or Telescope buffers `<leader><leader>` then `<C-v>`
- Swap windows’ contents: `<C-w>x`; lift current window to new tab: `:tab split`


## 3) Telescope — Files, Symbols, Search

Telescope is your fuzzy finder for files, buffers, text, and LSP symbols.

Keymaps
- `<leader>sf` — Find files
- `<leader>sh` — Help tags (Neovim docs)
- `<leader>sk` — Keymaps (discover bindings)
- `<leader>ss` — Telescope builtins menu
- `<leader>sw` — Grep word under cursor
- `<leader>sg` — Live grep project
- `<leader>sd` — Diagnostics picker (errors/warnings)
- `<leader>sr` — Resume last picker
- `<leader>s.` — Recent files (oldfiles)
- `<leader><leader>` — Buffers list
- `<leader>/` — Fuzzy search in current buffer (dropdown theme)
- `<leader>s/` — Live grep in open files only
- `<leader>sn` — Find files in your Neovim config dir

Picker controls
- Insert mode (default): `<CR>` open, `<C-x>` split, `<C-v>` vsplit, `<C-t>` tab, `<C-/>` show keymaps
- Normal mode (press `<Esc>` inside): `o`/`<CR>` open, `s` split, `v` vsplit, `t` tab, `?` show keymaps

Open multiple at once
- In a picker: mark with `<Tab>`, send to quickfix with `<C-q>`, `:copen`, then:
  - Tabs: `:cfdo tabedit %`
  - Splits: `:cfdo vsplit | wincmd l`


## 4) LSP — Code Intelligence

You use two Python language servers:
- `ty` (Astral) — main analyzer for Python (hover, go‑to, diagnostics, inlay hints, etc.)
- `ruff` — linter/formatter LSP; its hover is disabled so `ty` handles hover

What LSP features mean
- Definition/Declaration — jump to where a symbol is defined (`grd`) or declared (`grD`)
- Type Definition — jump to the type of the symbol (`grt`)
- References — where the symbol is used (`grr`), across project
- Implementations — where an interface/abstract method is implemented (`gri`)
- Document Symbols — an outline (functions, classes, variables) within the current file (`gO`)
- Workspace Symbols — a project‑wide symbol index (`gW`) searchable by name
- Code Action — quick fixes/refactors for the current location or selection (`gra`)
- Rename — rename the symbol across files (`grn`)
- Inlay Hints — inline parameter names/types for readability; enabled automatically

Keymaps (buffer‑local on attach)
- `grn` — Rename symbol
- `gra` — Code action (also in visual mode)
- `grr` — References (Telescope)
- `gri` — Implementations (Telescope)
- `grd` — Definitions (Telescope)
- `grD` — Declaration
- `gO` — Document symbols (Telescope outline)
- `gW` — Workspace symbols (Telescope)
- `grt` — Type definitions (Telescope)
- `<leader>th` — Toggle inlay hints (auto‑enabled when supported)

Diagnostics
- Quick list of diagnostics: `<leader>q` (location list)
- Rich UI: Trouble — project `<leader>xx`, buffer `<leader>xw`

Tips
- After jumping with LSP, use `<C-o>` to go back, `<C-i>` forward
- Prefer Telescope views of LSP results to choose when multiple matches exist


## 5) Formatting — Conform + Ruff

Formatting is handled by Conform with sensible defaults:
- `<leader>f` — Format buffer (async, LSP fallback)
- Python: uses `ruff_fix` + `ruff_format` for speed and consistency
- Info/Debug: `:ConformInfo`


## 6) Virtualenv — venv‑selector

Quickly switch your interpreter so LSP, formatter, tests, and debugger all agree.
- `<leader>cv` — Choose Python venv (Telescope UI)
- Commands: `:VenvSelect`, `:VenvSelectCached`
- Tip: Keep a project‑local `.venv` to make auto‑detection and tooling consistent


## 7) Testing — neotest + pytest

Run and inspect tests without leaving Neovim. Neotest uses pytest and integrates with DAP.
- `<leader>tn` — Run nearest test
- `<leader>tf` — Run tests in current file
- `<leader>ts` — Toggle summary panel
- `<leader>to` — Toggle output panel
- `<leader>tw` — Watch current file (re‑run on save)

Config highlights
- Runner: `pytest`
- DAP integration: `justMyCode = false` (lets you debug into dependencies)


## 8) Debugging — nvim‑dap + dap‑ui + dap‑python

Interactive debugging with breakpoints, stepping, variables, stack traces.
- `<F5>` — Continue/start session
- `<F9>` — Toggle breakpoint
- `<F10>` — Step over
- `<F11>` — Step into
- `<F12>` — Step out
- `<leader>du` — Toggle DAP UI panels
- `<leader>dr` — Open DAP REPL

Notes
- Python adapter: `debugpy` (installed via Mason)
- dap‑python setup uses `python` from your current venv (switch via `<leader>cv` first)


## 9) Docstrings — neogen

Generate docstrings in Google style (configured for Python).
- `<leader>cg` — Generate docstring for function/class at cursor (uses `google_docstrings` template)
- Command: `:Neogen`


## 10) Markdown — inline rendering

Render Markdown inline (no browser/Node) via render‑markdown.nvim.
- `<leader>mr` — Toggle inline rendering (`:RenderMarkdown toggle`)
- Great for reading/writing docs inside Neovim; toggle off to edit raw markup


## 11) Buffer/Window/Tab Helper Keymaps (Quality of Life)

Buffers
- `<leader>bn` / `<leader>bp` — Next / previous buffer
- `<leader>bd` — Delete current buffer
- `<leader>bo` — Keep only current buffer (close others)
- `<leader>a` — Alternate buffer (same as `:b#`)

Windows
- `<leader>wv` — Vertical split; `<leader>ws` — Horizontal split
- `<leader>wc` — Close current window; `<leader>wo` — Keep only this window
- `<leader>w=` — Equalize window sizes (same as `<C-w>=`)

Tabs
- `<leader>ta` — New tab; `<leader>tc` — Close tab; `<leader>to` — Tab only
- `<leader>t1..t9` — Jump to tab 1..9
- Native navigation: `gt` / `gT`, go to Nth tab: `{N}gt`


## 12) Quickfix and Location Lists

- Quickfix (global): `:copen`/`:cclose`, `:cnext`/`:cprev`, first/last: `:cfirst`/`:clast`
- Location list (window‑local): `:lopen`/`:lclose`, `:lnext`/`:lprev`, first/last: `:lfirst`/`:llast`
- From any Telescope picker: `<C-q>` sends selections to quickfix for batch open


## 13) Advanced Editing Essentials

- Change/delete/yank/paste: `c/D/d/y/p/P`, line ops `dd/yy`
- Indent: `>>` / `<<` (visual `=` to reindent)
- Case: `gU` upper, `gu` lower (e.g., `gUw`)
- Join lines: `J` (keeps space) / `gJ` (no space)
- Replace: `r{char}` single, `R` replace mode
- Visual block edits: `<C-v>` then `I`/`A` to insert/append on all selected lines

Macros and repeat
- Record: `q{register}` … `q`; play: `@{register}`, repeat last: `@@`
- Dot repeat: `.` repeats the last change (amplify with text objects)

Search, substitute, global
- Use `\v` at start for “very magic” regex (fewer escapes)
- Current line: `:s/old/new/g`; whole file: `:%s/old/new/gc` (confirm)
- Visual range substitute: visually select, then `:s/old/new/g`
- `:g/pattern/normal gw` — run Normal command on all matches

Registers and clipboard
- System clipboard: `"+` (yank `"+y`, paste `"+p`); black hole: `"_` (discard)
- View all registers: `:registers`

Marks and sessions
- Global marks (across files): uppercase `mA..mZ`, jump with `'A..'Z`
- Sessions: `:mksession! Session.vim` to save, `:source Session.vim` to load


## 14) Health, Tools, and Updates

- Health: `:checkhealth`
- Mason: `:Mason` (ensure tools: `ruff`, `ty`, `debugpy`)
- LSP clients: `:LspInfo`
- Conform settings: `:ConformInfo`
- Treesitter parsers: `:TSUpdate`
- Which‑key tips menu appears after you press `<leader>`


## 15) Reference Keymaps by Category (All in One)

Global
- `<Esc>` — Clear search highlights
- `<leader>q` — Diagnostic list (location list)
- `<Esc><Esc>` (t) — Exit terminal to Normal
- Window focus: `<C-h>`, `<C-j>`, `<C-k>`, `<C-l>`

Telescope
- `<leader>sf`/`sh`/`sk`/`ss`/`sw`/`sg`/`sd`/`sr`/`s.`/`<leader><leader>`/`/`/`s/`/`sn`

LSP
- `grn`, `gra`, `grr`, `gri`, `grd`, `grD`, `gO`, `gW`, `grt`, `<leader>th`

Formatting
- `<leader>f`

Diagnostics (Trouble)
- `<leader>xx`, `<leader>xw`

Venv
- `<leader>cv`

Testing
- `<leader>tn`, `<leader>tf`, `<leader>ts`, `<leader>to`, `<leader>tw`

Debugging
- `<F5>`, `<F9>`, `<F10>`, `<F11>`, `<F12>`, `<leader>du`, `<leader>dr`

Docstrings
- `<leader>cg`

Markdown
- `<leader>mr`

Buffers/Windows/Tabs
- `<leader>bn`, `<leader>bp`, `<leader>bd`, `<leader>bo`, `<leader>a`
- `<leader>wv`, `<leader>ws`, `<leader>wc`, `<leader>wo`, `<leader>w=`
- `<leader>ta`, `<leader>tc`, `<leader>to`, `<leader>t1..t9`


Master these and you’ll navigate, refactor, test, and debug at speed — all inside Neovim.
