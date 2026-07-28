---
title: Computing Ecosystem Literacy
date: 2026-07-28
tags: ['computing']
toc: true
draft: false
---

## Intro
University students and autodidacts learning computer science often dive straight into topics such as programming, algorithms, and machine learning from their first semester, but receive no introduction to setting up a proper development environment with all the tools needed to work efficiently. This know-how doesn't require a full-semester class of its own, so students are left with confusing information from arbitrary sources (i.e., every class provides different, incomplete, or conflicting information) about how to get on with their main tasks. This creates a lot of friction before they can even start. I'm writing this post to serve as onboarding material for newbies getting into programming and computer science. This is not a programming guide, though; there are other resources for that. MIT also has the "Missing Semester" seminar class for the same reason. I'm giving my shorter and slightly opinionated version here.

## Shell
The shell is more efficient than a GUI because it is programmable and automatable. If you have a long series of commands to execute, you can easily create a script to run them instead of making possibly hundreds of clicks in a GUI every time. Without a shell, many workflows would be nearly impossible. Most programming tools are command-line-only for this reason, as creating a GUI for everything would involve a lot of extra work for little benefit. You'll therefore have to learn the basics of terminal use. Windows users will have to use WSL, as much development software is not compatible with Windows and its shell.

Shell commands are just normal programs located in one of the directories listed in a special variable (`$PATH`) that the shell searches whenever you execute a command. They are often followed by input parameters or flags, which commonly use the `-` character to configure their behavior. This is a meta-guide, not a complete tutorial or cheat sheet, but you can get by with just these:

- `ls` to see the contents of the current directory
- `pwd` to see the current directory
- `cd` to change directories; `~` means your home directory, `.` means the current directory, and `..` means the parent directory
- `rm` to delete a file; use the `-r` flag for folders (this flag is used with other commands too)
- `mkdir` to create a new folder
- `mv` to move a file
- `cp` to copy a file
- `man` to learn about a command or its flags
- `>` and `<` to redirect the contents of files into commands and vice versa, and `|` to pipe the output of one command into the input of the next
- For working with remote computers, use `ssh` to connect to them and `scp` to move files between computers

Unix shell commands are highly composable: they can be combined in clever ways to accomplish almost any conceivable task. People used to spend a lot of time learning these tricks, but now we have agents to help us with them. For example, `|` makes commands like this possible:

`cat access.log | awk '{print $1}' | sort | uniq -c | sort -nr`

This passes the contents of `access.log` through five different commands to count occurrences of particular values in the file.

The takeaway is that if you have a task you want to accomplish—such as "find content x in files y, transform it, and make it brew me coffee while doing backflips"—there's probably a way to do it.

You can write Bash scripts that execute like complete programs, with `if`/`else` statements, loops, and functions, but I usually reach for Python or another programming language whenever I need anything more than a series of simple commands.

## Development environment
I'll simply explain which tools exist and are useful here; my personal setup recommendation will come at the end.

### Shell editor: Vim
You need a terminal editor to make quick edits to files. Vim is notorious for its steep learning curve, but that only applies if you want to learn everything. With this cheat sheet, which you can memorize in a minute, you can already edit faster than with a GUI editor.

Run `vim filename` to open a text file in Vim.

Vim uses three modes:

- `Esc` for `normal` mode: moving around files
- `i` for `insert` mode: actually typing text
- `v` for `visual` mode: selecting blocks of text

Useful shortcuts for normal and visual modes:

- `gg`: move to start of file
- `G`: move to end of file
- `w`: move to next word
- `:<line number>`: go to line number
- `Ctrl+d` and `Ctrl+u`: scroll down or up by half a page
- `u` and `Ctrl+r`: undo or redo
- `0` and `$`: move to start/end of line
- `o` and `O`: start typing on a new line below or above the current line
- `/` followed by a search pattern and `Enter`: search for a pattern; then use `n` or `N` for the next or previous occurrence
- **`:wq` or `ZZ`: save and exit.**
- **`:q` or `ZQ`: exit without saving. `:q!` to force.**

Copying and pasting:

- `d` to cut (commonly used for deleting); `x` to cut one character
- `y` copy
- `p` paste

Double them to apply them to a whole line (i.e., `dd` cuts a line).

These commands become powerful when you combine them with the motions above. For example, combine cut (`d`) with move to the next word (`w`) as `dw` to cut to the next word. Similarly, `dG` cuts to the end of the file, and `y$` copies to the end of the line. You can create incredibly specific combinations.

Again, as with shell commands, I recommend taking some time to search for a shortcut whenever you need one instead of forcing yourself to learn everything up front. Eventually, you'll pick up many shortcuts that will make you highly productive. Vim is powerful enough that many developers use it as their main editor or IDE. [This section](https://missing.csail.mit.edu/2026/development-environment/#Putting-it-all-together) gives some examples of how to perform specific file-editing tasks.

### Terminal multiplexers
> When using the command-line interface, you will often want to run more than one thing at once. For instance, you might want to run your editor and your program side by side. Although you can achieve this by opening new terminal windows, using a terminal multiplexer is a more versatile solution.

> Terminal multiplexers such as tmux let you divide terminal windows into panes and tabs, allowing you to interact efficiently with multiple shell sessions. Moreover, terminal multiplexers let you detach from a current terminal session and reattach to it later. This makes terminal multiplexers especially convenient when working with remote machines.

> tmux expects you to know its keybindings. They all have the form `<C-b> x`, which means (1) press Ctrl+b, (2) release Ctrl+b, and then (3) press x.

You usually need a few terminal panels open for every project—for example, to use Git, browse files, run back-end and front-end servers, and run tests. Terminal multiplexers make this more convenient than opening multiple terminal processes.

I will explain my multiplexer setup in the last section.

### IDEs
VS Code is the most popular editor/IDE. It differs from a simple editor by providing an integrated view of all the files in an open workspace, syntax highlighting, error detection, version control (Git; more on that later), and other conveniences such as a built-in terminal, buttons to run, build, or test your program, and a debugger. It has plugins that support many languages and workflows.

Most of these features can be replicated by adding plugins to Vim. Alternatively, now that agents write most of the code, you can ditch editors completely (except for quick edits) and work entirely from a terminal.

I still recommend it for most newbies because it isn't advisable to rely entirely on agentic coding from day one without knowing how to program.

### Language-specific tools
You should stay up to date on the tools used to develop software in your language of choice. These include package managers and build tools (which sometimes come with the language itself—by "language," we mean its compiler or interpreter), formatters, linters, static code-analysis tools ("static" in software means without running the program, as opposed to dynamic or runtime analysis), runtime-analysis tools and emulators (e.g., tools that identify which parts of the code are the slowest or least memory-efficient), and testing frameworks.

The code-quality checks listed above are especially important in the age of agentic computing. Agents are statistical machines with a limited understanding of whether code actually works, so we need to put them in a tight feedback loop to minimize errors. Newbies especially underestimate the value of tests. When you have a complete app with 50,000 lines of source code, you can't simply run it and visually determine whether a change has broken existing behavior; checking everything could take hours. A good test suite can be run quickly with a single command to verify that nothing has broken. The time savings are immense.

Package managers let you conveniently install dependencies (third-party libraries) in your project. One important concern is ensuring reproducible runs and builds. Your package manager makes sure you use the pinned version of every dependency (and its transitive dependencies) every time, so you don't suddenly encounter issues caused by incompatible versions of imported libraries.

For Python, use `uv` as the package manager, `ruff` as the code formatter, and `mypy` with a strict configuration file for static type checking.

## Version control and Git
> Version control systems (VCSs) are tools used to track changes to source code (or other collections of files and folders). As the name implies, these tools help maintain a history of changes; furthermore, they facilitate collaboration. Logically, VCSs track changes to a folder and its contents in a series of snapshots, where each snapshot encapsulates the entire state of files/folders within a top-level directory. VCSs also maintain metadata like who created each snapshot, messages associated with each snapshot, and so on.

> Why is version control useful? Even when you’re working by yourself, it can let you look at old snapshots of a project, keep a log of why certain changes were made, work on parallel branches of development, and much more. When working with others, it’s an invaluable tool for seeing what other people have changed, as well as resolving conflicts in concurrent development.

Many newbies initially think it's needless bureaucracy to periodically stage and commit their code changes to a Git repository, but I guarantee that you will regret not using Git. It allows you to keep a history of all your changes and revert to any of them—like save-scumming in a video game whenever you make errors you don't know how to recover from. It also allows multiple people (or agents) to work on the same codebase. Without the first feature, you'd have to keep a new "Save As" copy of the whole repository every time you made a change. Without the second, you'd have to assign specific files to everyone involved, have them send you the resulting files, and manually overwrite your local copies, assuming there were no merge conflicts (i.e., everyone worked on completely different files). Git makes all of that easy.

The MIT class and most tutorials are very good at overcomplicating Git workflows. However, Git is very simple if you ignore the more complex commands.

1. `git init` initializes an empty repository in the current directory.
2. `git add <files>` stages any changes you want to include in the next commit (commonly `git add .` to add everything). Staging is not permanent and can be undone or redone.
3. `git commit -m "<commit message here>"` commits your staged changes. This creates an immutable "save file" of your repository at this stage.

That's it. This might account for 90% of your Git usage if you work alone. You can add a "remote repository" to keep your code outside your PC by creating a repository on GitHub, setting it as the remote for your local repository, and then running `git push`. Pushing sends all your commits. `git pull` downloads and integrates commits from the remote repository that you don't yet have locally. `git clone` is like pulling, but it is used when you don't yet have the repository on your computer.

You will commonly use `git status` to see the repository's current status and its staged and unstaged files, `git diff` to show your changes since the last commit, and `git log` to show the commit history. `git checkout` has many uses, but one common use is returning to a previous commit. You can even selectively undo a past commit without also undoing changes that occurred after it by using `git revert`.

The workflow for making changes to a repository that multiple people or agents are actively working on involves using Git branches. Pull the latest changes, then:

1. Run `git checkout -b "<new branch name>"` to create a new branch and check it out.
2. Add commits to this branch in the usual way with `git add` and `git commit`, and optionally use `git push`.
3. Merge these commits back into the main tree by checking it out with `git checkout main`, pulling the latest changes, and then running `git merge "<branch name>"`.

The working tree might have diverged since you created your branch, but that won't necessarily be a problem. If it has diverged *and* the same parts of the same files were changed both in the working tree and in your branch, Git will guide you through resolving the merge conflicts.

This is also the workflow for contributing to other people's projects, except that you first fork the project on GitHub and clone the forked repository.

Remember that not every file should be added to Git. The `.gitignore` file tells Git to ignore files such as build artifacts, local development settings, passwords or secrets, and large data files that should be downloaded or generated separately. Luckily, agents are good at creating this file.

## Agentic coding
Use the CLI agent tool from your preferred provider. OpenAI, Anthropic, and some Chinese models currently provide the best coding agents. LLMs have become good enough that, for the most part, you no longer need to code by hand. They do still need some steering and guidance, though.

I start new projects by writing a specification that describes how the project should work and which tech stack should be used. I then have the agent ask me about any design decisions that need to be made or anything else that isn't clear before letting it create an implementation plan. You need to make sure that this implementation plan makes sense and that your specification actually matches what you want to implement.

- `AGENTS.md` can be used to specify the tech stack, programming guidelines, and overall workflow for the agent. I make sure that it uses my preferred libraries and follows good coding standards, such as using proper data structures instead of stringly typed variables or magic values. I also have it implement strict unit tests or even [property-based tests](https://hypothesis.readthedocs.io/en/latest/) and run them alongside static code-analysis tools every time it makes a change.
- When the implementation is complete, ask the agent to analyze it for bugs, performance problems, security issues, and inefficient, overengineered, or dead code.
- I use a high reasoning level for planning or analysis and a medium level for most implementation work.
- Try not to overload the context, and clear it often, especially when moving, for example, from the back end to the front end.

## Current workflow
Until recently, my workflow for every project involved:

1. A terminal with a CLI agent for most of the coding
2. VS Code, mostly for looking through the code—especially the Git tab, which provides a convenient view of the diffs—even though I don't personally edit much
3. A few more terminals in tmux for running, building, and testing

Now that agentic coding has progressed to the point where you rarely need to write code by hand, I have improved my workflow by ditching VS Code, since it isn't really needed anymore. This accomplishes two things: there's no need to have a VS Code process open for every project, and all our tools are fully command-line-based.

Since we're working entirely from the command line, we can put everything into our terminal multiplexer. I use [`herdr`](https://herdr.dev/), which is a better terminal multiplexer than tmux (although both are good), especially for agentic coding.

My `herdr` setup:

1. Use a workspace for every project or repository.
2. Use a few tabs in every workspace:
   - A CLI agent tab
   - A "main" tab for reviewing diffs (with [`hunk`](https://www.hunk.dev/)), running Git commands, and performing general file operations
   - Additional tabs (possibly with split panels) for running, building, and testing each front end, back end, worker, and so on

Bonus: since we work entirely from the command line and don't often type code by hand, the Herdr developers had the smart idea of making their terminal UI mobile-friendly. This means we can connect to our computer using Tailscale and `ssh` and work from a mobile phone without much inconvenience. When you connect from your phone using `ssh` and run `herdr`, it instantly brings up all your open projects and tabs. You can detach at any time, leave them running as they are, and reconnect later.
