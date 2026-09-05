---
title: "tmux Cheatsheet"
date: 2026-09-05
tags: [tmux, terminal, tips]
permalink: /snippets/tmux-cheatsheet/
---

A compact reference for everyday tmux usage.

**Prefix:** `<C-b>` means press `Ctrl-b`, release it, then press the following key.

## 1) Sessions

| Command | Description |
| --- | --- |
| `tmux` | Start tmux |
| `tmux new -s <name>` | Create a named session |
| `tmux ls` | List sessions |
| `tmux attach` | Attach to the most recently used unattached session |
| `tmux attach -t <name>` | Attach to a named session |
| `<C-b> d` | Detach from the current session |
| `<C-b> s` | Choose a session interactively |
| `tmux kill-session -t <name>` | Kill a named session |

Example:

```bash
cd ~/repos/my-project
tmux new -s my-project
```

Later:

```bash
tmux attach -t my-project
```

## 2) Windows

A **window** is similar to a tab. A session can contain multiple windows.

| Keys / Command | Description |
| --- | --- |
| `<C-b> c` | Create a new window |
| `<C-b> n` | Next window |
| `<C-b> p` | Previous window |
| `<C-b> w` | Choose a window interactively |
| `<C-b> 0..9` | Jump to window by number |
| `<C-b> ,` | Rename current window |
| `<C-b> &` | Kill current window |

Example project:

```text
0: editor
1: shell
2: tests
3: server
```

## 3) Panes

A **pane** is a terminal region inside a window.

| Keys | Description |
| --- | --- |
| `<C-b> %` | Split into left/right panes |
| `<C-b> "` | Split into top/bottom panes |
| `<C-b> ←/→/↑/↓` | Move between panes |
| `<C-b> o` | Move to next pane |
| `<C-b> q` | Show pane numbers |
| `<C-b> q <number>` | Jump to numbered pane |
| `<C-b> x` | Kill current pane |
| `<C-b> z` | Zoom/unzoom current pane |
| `<C-b> !` | Move current pane into its own window |

Typical layout:

```text
┌────────────────────┬────────────────────┐
│                    │                    │
│       editor       │       shell        │
│                    │                    │
├────────────────────┴────────────────────┤
│                 tests                   │
└─────────────────────────────────────────┘
```

## 4) Resizing Panes

| Keys | Description |
| --- | --- |
| `<C-b> <C-←>` | Resize pane left |
| `<C-b> <C-→>` | Resize pane right |
| `<C-b> <C-↑>` | Resize pane up |
| `<C-b> <C-↓>` | Resize pane down |
| `<C-b> Space` | Cycle through predefined layouts |

## 5) Scrolling and Copy Mode

Normal terminal scrolling behaves differently inside tmux. tmux maintains its own history.

| Keys | Description |
| --- | --- |
| `<C-b> [` | Enter copy mode |
| `↑` / `↓` | Move through history |
| `Page Up` / `Page Down` | Move through history faster |
| `q` | Exit copy mode |

Copy mode is also where tmux provides keyboard-based selection and copying. Exact keys depend on whether tmux is using Emacs or vi-style copy-mode bindings.

## 6) Command Mode

| Keys | Description |
| --- | --- |
| `<C-b> :` | Open tmux command prompt |

You can then execute tmux commands directly:

```text
new-window
split-window
rename-window editor
kill-pane
```

Press `<Enter>` to execute the command.

## 7) Help

| Keys / Command | Description |
| --- | --- |
| `<C-b> ?` | Show all current key bindings |
| `<C-b> :`, then `list-keys` | List key bindings from command mode |
| `tmux list-commands` | List available tmux commands |
| `man tmux` | Open the tmux manual |

When you forget a shortcut, `<C-b> ?` is usually the best place to start.

## 8) Session Workflow

A simple project-per-session workflow:

```bash
cd ~/repos/my-project
tmux new -s my-project
```

Work normally:

```text
my-project
│
├── 0: editor
├── 1: shell
├── 2: tests
└── 3: server
```

When finished for now:

```text
<C-b> d
```

The session continues running.

Later:

```bash
tmux ls
tmux attach -t my-project
```

A useful mental model is:

```text
tmux
│
├── session
│   │
│   ├── window
│   │   ├── pane
│   │   └── pane
│   │
│   └── window
│       └── pane
│
└── session
    └── window
        └── pane
```

## 9) Quick Reference

| Category | Commands |
| --- | --- |
| Start | `tmux`, `tmux new -s <name>` |
| Sessions | `tmux ls`, `tmux attach -t <name>`, `<C-b> d`, `<C-b> s` |
| Windows | `<C-b> c`, `n`, `p`, `w`, `0..9`, `,`, `&` |
| Panes | `<C-b> %`, `"`, arrows, `o`, `q`, `x`, `z` |
| Resize | `<C-b> <C-arrow>` |
| Scroll | `<C-b> [` |
| Command mode | `<C-b> :` |
| Help | `<C-b> ?`, `man tmux` |

## 10) Commands Worth Memorizing First

You do not need to memorize everything above.

Start with:

```text
tmux new -s <name>       Create session
tmux ls                  List sessions
tmux attach -t <name>    Reattach

<C-b> d                  Detach

<C-b> c                  New window
<C-b> n                  Next window
<C-b> p                  Previous window

<C-b> %                  Split left/right
<C-b> "                  Split top/bottom
<C-b> arrow              Change pane
<C-b> x                  Close pane
<C-b> z                  Zoom pane

<C-b> [                  Scroll/copy mode
<C-b> ?                  Help
```

