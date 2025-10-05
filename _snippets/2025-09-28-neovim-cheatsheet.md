---
title: "Python Neovim cheatsheet"
date: 2025-09-28
tags: [python, neovim, tips]
permalink: /snippets/neovim-cheatsheet/
---

This is the single, comprehensive guide for mastering Neovim itself and the Python‑focused setup in your `init.lua`. It combines fundamentals, advanced workflows, and keymaps from your configuration with clear explanations.

Legend: `<leader>` is space. Modes: `n` = normal, `t` = terminal, `x` = visual.


## 1) Basics

| Keys / Command | Description |
| --- | --- |
| `w`/`b`/`e` (`W`/`B`/`E`) | Move word/WORD forward/back/end |
| `0`/`^` / `$` | Line start (0/^), line end ($) |
| `gg` / `G` | Go to top / bottom of file |
| `H` / `M` / `L` | Jump to top/middle/bottom of screen |
| `<C-u>` / `<C-d>` | Half‑page up / down |
| `<C-f>` / `<C-b>` | Page down / up |
| `%` | Jump between matching pairs `()[]{}` |
| `/pattern` / `?pattern` | Search forward / backward; navigate with `n` / `N` |
| `<Esc>` | Clear search highlight (mapped) |
| `<C-o>` / `<C-i>` | Jumplist back / forward |
| `<C-^>` or `:b#` | Switch to alternate buffer |
| `ma` then `'a` or `` `a`` | Set and jump to mark a; special: ```` (last jump), `'.` (last edit) |
| `v` / `V` / `<C-v>` | Visual char / line / block mode |
| Text objects | `iw/aw`, `i'/a'`, `i"/a"`, `i(`/`a(`, `i{`/`a{`, `ip/ap` |
| Operators + text obj | e.g., `ci"`, `da)`, `viw` |
| Folds | `za` toggle, `zo`/`zc`, `zR`/`zM`, `zr`/`zm` |
| `:e path` | Edit file (with completion) |
| `:terminal` | Open terminal; exit to Normal: `<Esc><Esc>` (mapped) |
| `:w`/`:wa` / `:q`/`:qa` / `:wqa` | Write/write all; quit/quit all; write+quit all |


## 2) Windows, Buffers, Tabs

| Keys / Command | Description |
| --- | --- |
| `<C-h>`, `<C-j>`, `<C-k>`, `<C-l>` | Move focus left/down/up/right (mapped) |
| `:split` / `:vsplit` | Open horizontal / vertical split |
| `<leader>ws` / `<leader>wv` | Open split via mapping |
| `<C-w>=` or `<leader>w=` | Equalize split sizes |
| `:q` or `<leader>wc` | Close current window |
| `:only` or `<leader>wo` | Keep only current window |
| `:tabnew` / `:tabclose` | New / close tab |
| `<leader>ta` / `<leader>tc` / `<leader>to` | Tab new / close / only |
| `gt` / `gT` / `{N}gt` | Next / previous / jump to Nth tab |
| `:ls` | List buffers |
| `<leader>bn` / `<leader>bp` | Next / previous buffer |
| `<leader>bd` | Delete buffer |
| `<leader>a` or `:b#` | Alternate buffer (toggle last) |
| `<leader>bo` | Keep only current buffer (close others) |

Tips
- From Telescope, open right split: `<leader>sf` → pick → `<C-v>`
- Keep a “home” file on left; explore right; flip last buffer with `<leader>a`
- Swap windows: `<C-w>x`; lift to new tab: `:tab split`


## 3) Telescope (Files, Symbols, Search)

| Keys / Command | Description |
| --- | --- |
| `<leader>sf` | Find files |
| `<leader>sh` | Help tags |
| `<leader>sk` | Keymaps picker |
| `<leader>ss` | Telescope builtins menu |
| `<leader>sw` | Grep word under cursor |
| `<leader>sg` | Live grep in project |
| `<leader>sd` | Diagnostics picker |
| `<leader>sr` | Resume last picker |
| `<leader>s.` | Recent files (oldfiles) |
| `<leader><leader>` | Buffers list |
| `<leader>/` | Fuzzy search in current buffer |
| `<leader>s/` | Live grep in open files only |
| `<leader>sn` | Find files in Neovim config dir |

Picker controls
- Insert: `<CR>` open, `<C-x>` split, `<C-v>` vsplit, `<C-t>` tab, `<C-/>` show keymaps
- Normal (press `<Esc>`): `o`/`<CR>` open, `s` split, `v` vsplit, `t` tab, `?` keymaps

Multi‑open
- Mark with `<Tab>`, send to quickfix `<C-q>`, `:copen` → `:cfdo tabedit %` or `:cfdo vsplit | wincmd l`


## 4) LSP (Code Intelligence)

| Keys / Command | Description |
| --- | --- |
| `grd` / `grD` | Go to definition / declaration |
| `grt` | Go to type definition |
| `grr` | References (Telescope) |
| `gri` | Implementations (Telescope) |
| `gO` | Document symbols (outline) |
| `gW` | Workspace symbols (project) |
| `gra` | Code action (also works in visual) |
| `grn` | Rename symbol |
| `<leader>th` | Toggle inlay hints |

Diagnostics
- Quick list: `<leader>q` (location list)
- Trouble: project `<leader>xx`, buffer `<leader>xw`

Notes
- Python LSPs: Astral `ty` (primary), `ruff` (lint/format hover disabled)
- Use `<C-o>` back / `<C-i>` forward after jumps


## 5) Formatting (Conform + Ruff)

| Keys / Command | Description |
| --- | --- |
| `<leader>f` | Format buffer (async, LSP fallback) |
| Python | `ruff_fix` + `ruff_format` configured |
| `:ConformInfo` | Inspect Conform settings |


## 6) Virtualenv (venv‑selector)

| Keys / Command | Description |
| --- | --- |
| `<leader>cv` | Choose Python venv (Telescope UI) |
| `:VenvSelect` / `:VenvSelectCached` | Select or reuse last venv |
| Tip | Prefer project `.venv` for consistent tooling |


## 7) Testing (neotest + pytest)

| Keys / Command | Description |
| --- | --- |
| `<leader>tn` | Run nearest test |
| `<leader>tf` | Run tests in current file |
| `<leader>ts` | Toggle summary panel |
| `<leader>to` | Toggle output panel |
| `<leader>tw` | Watch current file (re‑run on save) |
| Runner | `pytest` |
| DAP | `justMyCode = false` |

Pytest user commands (add to your `init.lua`)

## 8) Debugging (nvim‑dap + dap‑ui + dap‑python)

| Keys / Command | Description |
| --- | --- |
| `<leader>dc` | Continue/start (leader mapping) |
| `<leader>db` | Toggle breakpoint (leader mapping) |
| `<leader>do` | Step over (leader mapping) |
| `<leader>di` | Step into (leader mapping) |
| `<leader>dO` | Step out (leader mapping) |
| `<leader>du` | Toggle DAP UI panels |
| `<leader>dr` | Open DAP REPL |

Notes
- Python adapter: `debugpy` (via Mason)
- Uses interpreter from current venv (switch with `<leader>cv`)


## 9) Docstrings (neogen)

| Keys / Command | Description |
| --- | --- |
| `<leader>cg` | Generate docstring for function/class (Google style) |
| `:Neogen` | Run neogen |


## 10) Python REPL

| Keys / Command | Description |
| --- | --- |
| `<leader>rp` | New tab REPL for current file |
| `<leader>rS` | Split REPL for current file |


## 11) Markdown (inline rendering)

| Keys / Command | Description |
| --- | --- |
| `<leader>mr` | Toggle inline rendering |
| `:RenderMarkdown toggle` | Command behind the toggle |

Tip: Great for reading/writing docs inside Neovim; toggle off to edit raw markup.

## 12) File Explorer (oil.nvim)

| Keys / Command | Description |
| --- | --- |
| `<leader>e` | Open Oil in current working directory |
| `:Oil` | Open Oil in current directory |
| `:Oil .` | Open Oil at project/root path |
| In Oil: `<CR>` | Open file / enter directory |
| In Oil: `..` entry | Go to parent directory |
| (Optional) `-` mapping | Open parent directory into Oil buffer |
| `g?` (in Oil) | Show Oil help and available actions |


## 13) Buffer/Window/Tab Helpers

| Keys / Command | Description |
| --- | --- |
| `<leader>bn` / `<leader>bp` | Next / previous buffer |
| `<leader>bd` | Delete current buffer |
| `<leader>bo` | Keep only current buffer |
| `<leader>a` | Alternate buffer (like `:b#`) |
| `<leader>wv` / `<leader>ws` | Vertical / horizontal split |
| `<leader>wc` / `<leader>wo` | Close window / keep only this window |
| `<leader>w=` | Equalize window sizes |
| `<leader>ta` / `<leader>tc` / `<leader>to` | New tab / close / only |
| `<leader>t1..t9` | Jump to tab 1..9 |
| `gt` / `gT` / `{N}gt` | Native tab navigation / jump to Nth |


## 14) Quickfix and Location Lists

| Keys / Command | Description |
| --- | --- |
| `:copen` / `:cclose` | Open / close quickfix list |
| `:cnext` / `:cprev` | Next / previous quickfix item |
| `:cfirst` / `:clast` | First / last quickfix item |
| `:lopen` / `:lclose` | Open / close location list |
| `:lnext` / `:lprev` | Next / previous location item |
| `:lfirst` / `:llast` | First / last location item |
| `<C-q>` (Telescope) | Send selections to quickfix |


## 15) Editing Essentials

| Keys / Command | Description |
| --- | --- |
| `c`/`d`/`y`/`p`/`P` | Change/delete/yank/paste; line ops `dd`/`yy` |
| `>>` / `<<` / `=` (x) | Indent right / left / reindent visual |
| `gU` / `gu` | Uppercase / lowercase (e.g., `gUw`) |
| `J` / `gJ` | Join lines (with/without space) |
| `r{char}` / `R` | Replace single char / replace mode |
| `<C-v>` then `I`/`A` | Block insert / append |
| `q{reg}` … `q` / `@{reg}` / `@@` | Record / play macro / repeat last macro |
| `.` | Repeat last change |
| `\v` | Very‑magic regex mode |
| `:s/old/new/g` | Substitute on current line |
| `:%s/old/new/gc` | Substitute whole file with confirm |
| Visual + `:s/old/new/g` | Substitute in selection |
| `:g/pat/normal gw` | Run Normal cmd on all matches |
| `"+y` / `"+p` | System clipboard yank / paste |
| `"_` | Black‑hole register (discard) |
| `:registers` | View registers |
| `mA..mZ` / `'A..'Z` | Global marks set / jump |
| `:mksession!` / `:source` | Save / load session |


## 16) Health, Tools, Updates

| Keys / Command | Description |
| --- | --- |
| `:checkhealth` | Health checks |
| `:Mason` | Manage external tools (`ruff`, `ty`, `debugpy`, etc.) |
| `:LspInfo` | LSP clients status |
| `:ConformInfo` | Conform settings |
| `:TSUpdate` | Update Treesitter parsers |
| which‑key | Tips menu after pressing `<leader>` |


## 17) Quick Reference

| Category | Highlights |
| --- | --- |
| Global | `<Esc>` clear hl; `<leader>q` diagnostics; `<Esc><Esc>` (t) exit terminal; window focus `<C-h/j/k/l>` |
| Telescope | `<leader>sf/sh/sk/ss/sw/sg/sd/sr/s./<leader><leader>/ / s/ / sn` |
| LSP | `grn`, `gra`, `grr`, `gri`, `grd`, `grD`, `gO`, `gW`, `grt`, `<leader>th` |
| Formatting | `<leader>f` |
| Diagnostics (Trouble) | `<leader>xx`, `<leader>xw` |
| Venv | `<leader>cv` |
| Testing | `<leader>tn`, `<leader>tf`, `<leader>ts`, `<leader>to`, `<leader>tw`, `:PytestPattern`, `:PytestAll` |
| Debugging | `<leader>dc/db/do/di/dO`, `<leader>du`, `<leader>dr` |
| Docstrings | `<leader>cg` |
| REPL | `<leader>rp`, `<leader>rS` |
| Markdown | `<leader>mr` |
| Buffers/Windows/Tabs | `<leader>bn/bp/bd/bo/a`, `<leader>wv/ws/wc/wo/w=`, `<leader>ta/tc/to/t1..t9` |

Master these and you’ll navigate, refactor, test, and debug at speed — all inside Neovim.
