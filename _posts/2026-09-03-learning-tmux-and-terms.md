---
layout: post
title: Learning tmux (and terminals/shells)
date: 2026-09-03
description: Peeking into tmux and investigating what terminals and shells are.
tags: [tmux, terminal, shell, unix]
image:
  path: /assets/images/learning-tmux/tmux-session-panes.png
  alt: "A tmux session displaying multiple terminal panes"
twitter:
  card: summary_large_image
redirect_from:
  - /blog/2026/09/03/tmux-and-term-shells
---

Now with agents, tmux seems to be more popular than ever, but it is a tool I have always wanted to explore. At first it might look like just a way to split a terminal, but there is more happening behind it. In this post I will try to understand what tmux is, what it is good for, and how terminals and shells fit into the picture.

## What is a terminal?

A terminal is a physical device to interact with a computer, think of a just a small keyboard and a screen you'd plug in into a giant rack. Nowadays this might be harder to imagine, as with laptops for example, the computer and terminal are together. That's why they are not real terminals per se (an actual device), but **terminal emulators**.

Now keep in mind that a single computer might serve multilple terminals.

![Simple terminal diagram]({{ '/assets/images/learning-tmux/simple-terminal-diagram.svg' | relative_url }}){: .center-img }


### Terminal ≠ shell

Whenever you open your laptop terminal you will (most of the time) see something like:

```shell
alejandro@macbook ~ %
```

Now there are two parts to distinguish working together here:

1. The terminal emulator that as explained before handles the text based interface: displays output from computer using the screen and takes input from the keyboard.
2. The shell; that let's you execute commands.

There are plenty of shells over there such as `zsh`, `bash`, `fish`, etc...

That means that are two different components and you can combine them, e.g use `iTerm2` as terminal emulator in one laptop and use `bash` as the shell and in your other laptop use the same emulator but `zsh` instead.

### What happens when I type a command?

Whenever you type a command (for example, to run a Python script), the terminal emulator sends your keystrokes through a pseudo-terminal to the foreground process, which is usually the shell:

![Simple command input and output diagram]({{ '/assets/images/learning-tmux/simple-input-output-command.svg' | relative_url }}){: .center-img }

This diagram is deliberately simplified. The terminal emulator does not execute the command: the shell reads and parses it, then launches the requested program. The shell, the Python program and tmux are all processes. While the Python program is running in the foreground, it receives input from the terminal and writes its output back through the pseudo-terminal for the terminal emulator to display.


## One layer deeper: TTYs vs PTYs

So far we have discussed two visible components: the terminal emulator and the shell. Modern Unix systems have another layer between them: a PTY, or pseudo-terminal. A PTY is a pair of endpoints. The terminal emulator controls the master side, while the shell or foreground program is connected to the slave side. Historically, Unix communicated with physical terminals through TTY devices; PTYs provide similar behavior in software. tmux uses them to give every pane its own terminal.

```plaintext
keyboard -> terminal emulator -> PTY master -> PTY slave -> foreground process
                                                               |
screen   <- terminal emulator <- PTY master <- PTY slave <-----+
```


## What does multiplexer really mean?

Multiplexing (outside tmux world) is a broader general concept in engineering which means multiple logical streams sharing an underlying resource/channel, somethingl like:

```plaintext
A ---\                                        /--- A
B ---|-----MULTIPLEXER --- shared channel ----|--- B
C ---/                                        \--- C
```


This is very common on network, I/O, etc... But now we can kinda of start thinking what a terminal multiplexer is and what tmux allows: manage multiple terminals through terminal connections/interfaces.

Multiplexing does not necessarily require the streams to be split again. A multiplexer allows multiple inputs to share one resource; a demultiplexer is only needed when the receiving side must recover the individual streams. In tmux, multiple terminal programs share one outside terminal. Showing several panes at once is a useful feature, but it is not what makes tmux a multiplexer.


## tmux

If we start then mixing all of the concepts we learnt from terminals, shells, processes, multiplexing and so on we can go from the simple terminal process:

![Simple tmux diagram]({{ '/assets/images/learning-tmux/simple-diagram.svg' | relative_url }}){: .center-img }

Now if we add multiplexing to the mix:

![Multiplexed terminal diagram]({{ '/assets/images/learning-tmux/terminal-mpexed.svg' | relative_url }}){: .center-img }

tmux uses a client-server architecture. The client runs inside the outside terminal and communicates with the tmux server through a Unix socket. The server owns the sessions, keeps track of their windows and panes, and gives each pane its own PTY and running program.

```plaintext
terminal emulator
    ↕ outside PTY
tmux client
    ↕ Unix socket
tmux server
    ├── pane PTY ↔ shell/program
    └── pane PTY ↔ shell/program
```

This separation is what allows the client to disappear while the server and the programs inside its panes keep running.

### The detach command

In the previous diagram, the terminal emulator reaches the session through the tmux client. Detaching removes that client connection without stopping the tmux server or the programs it manages:

![Detached tmux client example]({{ '/assets/images/learning-tmux/dettach-example.svg' | relative_url }}){: .center-img }

Although the client is gone, the tmux server still owns the session and keeps the programs in its panes running. You can attach a new client later and continue working, which is particularly useful for long-running tasks.

![Simple tmux client-server diagram]({{ '/assets/images/learning-tmux/simple-diagram-client-server.svg' | relative_url }}){: .center-img }

In a remote SSH setup, the terminal emulator and SSH client run on your local computer, while the shell and tmux server run on the remote machine. In a local tmux session, all of these components run on the same computer.

If the network connection drops, the SSH client disconnects, but a tmux server on the remote machine keeps its sessions running. After reconnecting with SSH, you can attach a new tmux client to the existing session. Without tmux or a similar tool, programs tied to the disconnected terminal may terminate or become inaccessible.


### Sessions, windows and panes

`tmux` can be, as a starting point, split into these three pieces following a hierarchy:

1. Session: A persistent workspace that groups one or more windows and can have zero or more clients attached.
2. Window: A screen within a session that contains one or more panes. It is similar to a tab in a terminal emulator.
3. Pane: A rectangular region within a window containing its own terminal and running program.

You can imagine something as:

```plaintext
tmux
│
└── session: my-project
    │
    ├── window: editor
    │   ├── pane: vim
    │   └── pane: shell
    │
    ├── window: server
    │   └── pane: python app.py
    │
    └── window: database
        └── pane: psql
```


### First practical session

Make sure you follow the instructions to install `tmux` [from the official website](https://github.com/tmux/tmux/wiki/Installing).

![tmux session]({{ '/assets/images/learning-tmux/tmux-session.png' | relative_url }}){: .center-img }

Now, before digging deep you can start doing all kind of stuff with this simplified, check official page of commands [here](https://man.openbsd.org/tmux.1):


| Action               | Command                   |
| :------------------- | :------------------------ |
| Start tmux           | `tmux`                    |
| Create named session | `tmux new -s learning`    |
| List sessions        | `tmux ls`                 |
| Attach to session    | `tmux attach -t learning` |
| Detach               | `Ctrl-b d`                |
| New window           | `Ctrl-b c`                |
| Next window          | `Ctrl-b n`                |
| Previous window      | `Ctrl-b p`                |
| Choose window        | `Ctrl-b w`                |
| Split into left and right panes | `Ctrl-b %`       |
| Split into top and bottom panes | `Ctrl-b "`       |
| Next pane            | `Ctrl-b o`                |
| Show pane numbers    | `Ctrl-b q`                |
| Copy / scroll mode   | `Ctrl-b [`                |
| Help / key bindings  | `Ctrl-b ?`                |

The default tmux prefix is `Ctrl-b`: press `Ctrl` and `b` together, release them, and then press the key for the command you want. For example, `Ctrl-b c` creates a new window.

For example opening multiple panes:

![tmux session with multiple panes]({{ '/assets/images/learning-tmux/tmux-session-panes.png' | relative_url }}){: .center-img }


## Is tmux worth it?


Now you could have a tmux session for a specific repository, attach and detach from it, and keep its processes running on an external machine. tmux makes a lot of sense on remote machines because, if the connection is interrupted, the session keeps running and you can attach to it again later.

But tmux can also be useful on a modern laptop. Your workflow is no longer tied to a particular terminal window, and you can keep a persistent workspace for each project. Additionally, tmux offers plenty of configuration, similar to Vim, to speed up the way you work with sessions, windows and panes.

I'd say tmux is worth trying if some of these apply to you:

- You use SSH frequently.
- You run long-running processes.
- You enjoy keyboard-driven terminal workflows.
- You like to organize and persist project workspaces.
- You often keep many shells open.

## Resources

- [tmux Cheatsheet]({{ '/snippets/tmux-cheatsheet/' | relative_url }}).
- [Official tmux Getting Started guide](https://github.com/tmux/tmux/wiki/Getting-Started).
- [tmux manpage](https://man.openbsd.org/tmux.1).
