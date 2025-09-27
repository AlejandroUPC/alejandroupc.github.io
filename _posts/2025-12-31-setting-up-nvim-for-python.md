---
layout: post
title: Setting up Nvim for Python
date: 2024-09-24
tags: [python, neovim, lua]
permalink: /blog/2025/09/24/nvim-for-python
---

# Setting up Neovim for Python

**Disclaimer** by no means I am a Neovim expert and might not fully understand all of the internals of the tools I am going to talk about, this is just me learning by setting up a Nvim installation for Python.

I have used nvim in the past, I use it often (like writing this blog) but when coding I still use vscode as it has a few features that I find useful such as the UI for tests, environment/run configurations and debugging.
Regardless, I use the nvim extension for the motions in VSCode. I think Neovim, besides making you very efficient using the keyboard, its kind of a game, while you code.

I'd recommend everyone trying to learn it, there's plenty of resources and people in the [additional resources](#additional-resources) sections from people who knows a lot and have great tutorials.

## Introduction

Neovim (an extension of another text editor, vim) is a relatively simple (by default) text editor that is thought to be used mostly just using the keyboard. Over the time it has gained popularity as it allows (with time and practice) for incredible efficiency to code.

If its so good why is not that popular though? Well it has a kind of learning curve, it can be somewhat hard to maintain (when you extend functionalities with plugins).

I really recommend that if you come across this post and have not tried yet, give it a shot and jump directly to any tutorial and if you decide to embrace the journey you will find yourself creating something very personal, your own config that allows you to be very effective when navigating and working with code.

## Installation

This is a guide on how to install Neovim on a MacOS M3, but it should work pretty much on any Linux distro. You could just copy in your terminal:

```sh
brew install neovim ripgrep fd
```
The first package neovim is quite self explanaotry, and then ripgrep and fd are improvements of grep and find, respectively, that are faster. The first one is to find text in files and the second one is to find files (if you are not familiar with those two commons I would recommend googling a little bit around).
If you run then in your terminal `nvim` you should be greeted by a very simple window:
![Neovim start]({{ '/assets/images/00_nvim_intro.png' | relative_url }})

And this would be the simplest version of Neovim, if you run `neovim test.txt` this will create a file (will not be saved by default) where you could start writing, but lets start for one of the hardest parts in Neovim; how to close it? First press `esc` to make sure you are in normal mode and then:

1. `:q` -> Will quit if there are no unsaved changes.
2. `:q!` -> Force quit, without saving.
3. `:wq` or `:x` -> save and quit.
4. `ZZ` -> write and quit.

![Exiting Neovim]({{ '/assets/images/01_exit_working_quit.png' | relative_url }}){: .d-block .mx-auto}

Note that there are several ways more, but those are the ones I have used over time, note that for the ones starting with semicolon `:` is that it will show at the bottom right the editor the characters you enter and then just press enter when you are done.

In order to open neovim in an existing directory you can just go `nvim (path)` which I usally cd in to the project and write `nvim .`.


## Moving forward 

We are moving now a leap forward and assuming you are familiar with neovim modes and concepts such as buffers, windows, tabs... And jumping directly to [TJ DeVries video - The Only Video You need to Get Started With Neovim](https://www.youtube.com/watch?v=m8C0Cq9Uv9o).

It basically lets you set up a pretty complete set up using kickstart.nvim and ge you started, it is very important that you watch the video and google a little bit around, play around with the `:Tutor` and watch a few videos.

If you ever feel stuck or feel like a feature is missing as we progress in this post, investigate and see how feasible is to implemenet it (it probably is, the question is how hard).

## Setting up a Python Nvim installation

Using `Lazy` its quite easy, we are looking for the following features in our IDE:

1. Linter: We use `ruff` from astral.
2. Typing: We use `ty` from astral.
3. Support for testing.
4. Support for debugging.
5. Manage virtual environments


### Linter and typing

We will be tackling those two at once given that they are kinda related, one is going to make our code look pretty and the other make sure the types in our code match the signature of our methods.

This should be quite easy and wherever we are declaring all of our plugins using `Lazy`, you should look something like `require('lazy').setup` in your code, in the server table:

```lua
        ruff = {
          on_attach = function(c)
            c.server_capabilities.hoverProvider = false -- ty takes over hover
          end,
        },
        ty = {},

```

This will just make sure that both ruff and ty are set up in our LSP, we just set the hover priority over `ty` for types rather than `ruff`. Other than that you can find the lsp configurations for each of the plugins and see all the options that can be passed.

Really the work of `ruff` is mostly automatic and it will basically format on save or when pressing `leader+f`. For `ty` we will be getting warnings whenever, for example, there is a type missmatch:

![Ty Typing]({{ '/assets/images/03_ty_typin.png' | relative_url }}){: .d-block .mx-auto}


### Testing

For testing I really wanted a visual way to understand how the tests are doing and the output of the testing incase it fails, this really comes out of the box for `neotest` and `neotest-python`, for example with such a dummy test files if we just push `leader+t+f` as per test file we can see the status of the tests on the left:

![Failed test on file]({{ '/assets/images/04_failing_test.png' | relative_url }}){: .d-block .mx-auto}


Also you can do test summary with `leader+t+s` to see this fancy tab (see the pattern of leader+keystrokes that match our intention?):

![Test resume]({{ '/assets/images/05_test_resume.png' | relative_url }}){: .d-block .mx-auto}


### Debugging

Debugging is critical, very similar, at any point in code if we push `leader+d+b` it toggles a breakpoint, see the small `B` on the left:

![Add break point]({{ '/assets/images/06_debug_breakpoint.png' | relative_url }}){: .d-block .mx-auto}

Then if we just launch a session `leader+d+c` and it will stop and you will be able to explore the entire debug session:

![Debug session]({{ '/assets/images/07_debug_session.png' | relative_url }}){: .d-block .mx-auto}

### Virtual Environment

Quite simple, pressing `leader+c+v` will prompt you for which venv to use:

![Debug session]({{ '/assets/images/08_venv_select.png' | relative_url }}){: .d-block .mx-auto}

## Going forward

This was just a simple guide of someone who is kinda familiar with Neovim, if you decide to reuse the `init.lua` I have on my repo --TODO add link, I also recommend checking on snippets the neovim one to get familiar with the commands.


I probably also need to post a second part talking about oil, but please try Neovim and have the cheathseet opened in another tab to have quick access to the commands.


Again, Neovim can be hard at the beginning, the set up and the motions but I encourage you to give it a shot, if you come from VSCode enable `vim` mode there and try to use neovim later on directly.



### Additional resources

- [Neovim official page](https://neovim.io/)
- [Teej Dv Youtube chanel](https://www.youtube.com/@teej_dv/videos)
- [Telescope nvim](https://www.youtube.com/watch?v=iqdCshrIKIg)
