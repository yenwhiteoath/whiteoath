---
title: Computing Ecosystem Literacy
date: 2026-28-07
tags: ['computing']
draft: false
---

## Intro
University students and autodidacts learning computer science often dive immediately into the topics of programming, algorithms, machine learning etc. from the first semester, but have had no introduction on how to actually setup a proper development environment with all tools needed for efficient productivity. This know-how doesn't deserve its own full semester class, so students are left with confusing information from arbitrary sources (ie. every class providing different incomplete or conflicting information) on how to get on with their main tasks, accumulating a lot of friction before they can even start. I'm writing this post for this reason, as 'on-boarding' to help newbs get into programming and computer science. This is not a programming guide though, there are other resources for that. Also, MIT has the 'missing semester' seminar class for thr same reason, I'm giving my shorter and slightly opiniomated version here.

## Shell
Shell is more efficient than GUI because it is programmable/automatable. If you had a big series of commands to execute, you can easily create a script to execute them instead of doing possibly hundreds of clicks on a GUI every time, and many workflows would be almost impossible. Most tools in programming are purely shell for this reason, as creating a GUI for everything would be a lot of extra work for no reason. So you'll have to learn the basics of terminal use. Windows users will have to use `wsl`, as most software is not compatible with Windows and its shell.

Shell commands are just normal programs that are in a specific path ($PATH) the terminal looks for everytime you execute. They are often followed by their input parameters or flags using the - character to set up their settings. This is a meta-guide not a complete tutorial or a cheatsheet but you can get by just knowing these:

- `ls` to see contents of current directory. 
- `pwd` to see the current directory
- `cd` to change directory. `~` means home, `.` means current, `..` means above current.
- `rm` to delete file. `-r` flag for folders (used in other commands too)
- `mkdir` to creat a new folder
- `mv` to move file
- `cp` to copy file
- `man` to learn about a command or its flags
- `>` and `<` redirect contents of files into commands and vice versa, `|` to pipe the output of one command to the input of the next
- for working with remote computers, `ssh` to connect to them, `scp` to move files between computers

Unix shell commands are very composable, they can be combined in smart ways to accomplish every thinkable task. People used to spend a lot of time learning these tricks, but now we have agents to do that for us. For example, because of `|`, tricks like this are possible:

`cat access.log | awk '{print $1}' | sort | uniq -c | sort -nr`

This passes the contents of the file `access.log` through 5 different commands to count occurences of some contents in the file. `

The takeaway is, if you have some task you want to accomplish like "find me x content in y files, transform it, make it brew me coffee while doing backflips", there's probably a way to accomplish it.

You can write bash in script files to execute like complete programs, with if/else statements and loops and functions, but I usually reach for Python or another programming language whenever I want anything more than a series of simple commands to execute.

## Development environment
I'll simply explain what exists and is useful here, my personal recommendation for a setup will come at the end.

### Shell editor: vim
You need a terminal editor for making quick edits on files.
Notorious for its steep learning curve but only if you want to learn everything, with this cheatsheet that you can memorize in a minute you already edit faster than a GUI editor.

`vim filename` to open a text file in vim.

Vim uses 3 modes:
- `ESC` for `normal` mode: moving around files
- `i` for `insert` mode: actually typing text
- `v` for `visual` mode: selecting blocks of text

Useful shortcuts for normal and visual mode:
- `gg`: move to start of file
- `G`: move to end of file
- `w`: move to next word
- `:<line number>`: go to line number
- `Cntrl+d` and `Cntrl+u`: scroll down/up by half a page
- `u` and `Cntrl+r`: undo/redo
- `0` and `$`: move to start/end of line
- `o` and `O`: start typing from below/above line
- `/` followed by search pattern + `enter`: search for pattern. Then `n` or `N` for next/previous occurence
- **`:wq` or `ZZ`: save and exit.**
- **`:q` or `ZQ`: exit without saving. `:q!` to force.**
  
Copy/pasting:
- `d` cut (commonly used for deleting). `x` to cut 1 character
- `y` copy
- `p` paste

Double them for whole line (ie. `dd` to cut a line).

The way these become powerful is by combining them with the motions above. For example, combine cut `d` with move to next word `w` as `dw` to cut until next word, `dG` until end of file, `y$` copies until end of the line, etc. There's insanely specific combinations you can do.

Again, as with shell commands, it is advised to just take some time searching for a shortcut for whatever you want to do instead of forcing yourself to read everything upfront, eventually you'll pick up a lot of shortcuts that will make you very productive. Vim is powerful enough that many developers use it as their main editor/IDE. [This section](https://missing.csail.mit.edu/2026/development-environment/#Putting-it-all-together) has some examples of how would you go about editing specific examples of files. 

### Terminal multiplexers
> When using the command line interface you will often want to run more than one thing at once. For instance, you might want to run your editor and your program side by side. Although this can be achieved by opening new terminal windows, using a terminal multiplexer is a more versatile solution.

> Terminal multiplexers like tmux allow you to multiplex terminal windows using panes and tabs so you can interact with multiple shell sessions in an efficient manner. Moreover, terminal multiplexers let you detach a current terminal session and reattach at some point later in time. Because of this, terminal multiplexers are really convenient when working with remote machines.

> tmux expects you to know its keybindings, and they all have the form <C-b> x where that means (1) press Ctrl+b, (2) release Ctrl+b, and then (3) press x.

You usually need a few terminal panels open for every project, like for using git, browsing through files, running backend and frontend servers, running tests, etc. Terminal multiplexers make this more convenient instead of opening multiple terminal processes.

I will explain my multiplexer setup at the last section.

### IDEs
VScode is the most popular editor/IDE. It differs from a simple editor by having an integrated view of all files in the opened workspace, syntax highlighting and error detection, version control (git, more later), and other minor conveniences like a built-in terminal, buttons to run/build/test your program, debugger. It has plugins to support many languages and workflows.

Most of those can be replaced by adding plugins to vim, or now that agents write most of the code, just ditch the editors completely (except for quick edits) and work completely from a terminal.

I still recommend it for most newbies because it's not adviseable to go 100% into agentic coding from day one without knowing programming at all.

### Language-specific tools
You should be up to date with what tools are used when developing with your language of choice. These include package managers and build tools (sometimes come with the language itself, where by language we mean its compiler/interpreter), formatters, linters and static code analysis tools ('static' in software means before having to run the program, the opposite of dynamic/runtime), runtime analysis and emulators (eg. to analyze performance, which part of the code is the slowest or least memory efficient, etc.), testing frameworks.

The code quality checks listed above are especially important in the age of agentic computing, agents are just statistical machines with poor actual understanding of whether code actually works, we need to put them in a tight loop to minimize errors. Newbies especially underrate how useful tests are: when you have a complete app with a source code of 50k lines, you can't just run it and try to see with your eyes if a change you made has broken previous code, covering everything could take hours, with a good test suite you can quickly run with a single command and verify this way that nothing has broken. The benefits in time saved are immense.

Package managers let you conveniently install dependencies (3rd party libraries) to your project. A big issue is easily replicable runs/builds, your package manager makes sure you use the pinned version of every dependency (and their own recursive dependencies) every time, so you don't randomly start getting issues with incompatible versions of imported libraries.

For Python: use `uv` as the package manager, `ruff` as code formatter, `mypy` with a string configuration file for static type checking.

## Version control and git
> Version control systems (VCSs) are tools used to track changes to source code (or other collections of files and folders). As the name implies, these tools help maintain a history of changes; furthermore, they facilitate collaboration. Logically, VCSs track changes to a folder and its contents in a series of snapshots, where each snapshot encapsulates the entire state of files/folders within a top-level directory. VCSs also maintain metadata like who created each snapshot, messages associated with each snapshot, and so on.

> Why is version control useful? Even when you’re working by yourself, it can let you look at old snapshots of a project, keep a log of why certain changes were made, work on parallel branches of development, and much more. When working with others, it’s an invaluable tool for seeing what other people have changed, as well as resolving conflicts in concurrent development.

Many newbies initially think it's needless bureaucracy to have to periodically stage and commit their code changes to a git repository, but I guarantee you will regret not using git. It allows you to keep a history of all your changes (and revert back to any of them, like savescumming in a videogame every time you make errors you don't know how to recover from) and have multiple people (or agents) work on the same code base. For the first, you'd have to keep a new "save as" copy of whole repository every time you made a change, for the 2nd you'd have to make sure to assign specific files for everyone involved to work on, then they'd send you the result files and you'd manually overwrite the local ones. Assuming there's no merge conflicts (ie. you all work on completely different files). Git makes all of that easy.

The MIT class and most tutorials are very good at overcomplicating git workflows, however it is very simple if you ignore most complex commands.

1) `git init` to initialize an empty repository at current directory
3) `git add <files>` to stage any changes you want for the next commit (commonly `git add *` to add everything). This is not immutable and can be undone/redone
4) `git commit -m "<commit message here>"` to commit your staged changes. This creates an immutable 'save file' of your repository at this stage.

That's it. This might be 90% of your git usage if you work alone. You can add a 'remote repository' to keep your code outside your PC by initializing a repository on github, setting it as the remote repository of your local repository, and then doing `git push`. Push sends all your commits. `git pull` synchronizes any commits the remote repository has that you don't have downloaded yet to your local one. `git clone` is like pulling but for when you don't have the repo yet downloaded in your current computer.

You will commonly use `git status` to see current repository status, staged/unstaged files, `git diff` to show your changes since the last commit, `git log` to show commit history, `git checkout` for many uses but commonly for reverting to a previous commit. You can even selectively undo a past commit without also undoing changes that occured after it, using `git revert`.

The workflow for making changes to a repository actively worked on by multiple people/agents involves making use of git branches. Pull the latest changes, then:
1. `git checkout -b "<new branch name>"` to create new branch and checkout in it
2. add commits to this branch in the normal way with `git add`, `git commit`, optionally `git push`
3. merge these commits them back to the main tree by checking out in it with `git checkout main`, pull latest changes, then `git merge "<branch name>"`.

The working tree might have diverged since you branched out, but that won't be a problem. If it did diverge *and* also had changed the same files that you changed in the branch that you merged, then git will guide you through fixing the merge conflicts.

This is also the workflow to contribute to other people's projects, except you first fork them on github, and clone the forked repository.

Remember that not every file should be added to git. The file `.gitignore` is used to make git ignore files like build artifacts, local dev settings, passwords/secrets, large data files that should be downloaded/generated separately. Luckily agents are good at creating this file. 

## Agentic coding
Use the CLI agent tool of your preferred provider. OpenAI, Anthropic and some chinese models are currently the best coding agents. LLM's have become good enough that for the most part you don't need to code by hand anymore, they do need some steering and guidance though. 

I start new projects by writing a specification which describes how it works and what tech stack should be used, then let the agent create an implementation plan for it, after having it ask me any design decisions that need to be taken or anything else that's not clear. You need to make sure this implementation plan makes sense, and your specification actually matches what you want to implement.

- `AGENTS.md` can be used to indicate the tech stack, programming guidelines, and overall workflow to the agent. I make sure it uses my preferred libraries, good coding standards such as using proper data structures for the data instead of stringly typed variables or magic values, implementing strict unit tests or even [property tests](https://hypothesis.readthedocs.io/en/latest/) and run them along with static code analysis tools every time it makes a change
- When the implementation is complete, the agent should be asked to analyze it for bugs, performance issues, security issues, inefficient/overengineered/dead code 
- I use high thinking level when planning or analyzing and medium for most implementation
- Try to not overload context, and clean it often, especially when moving on eg. from backend to frontend

## Current workflow
Until recently, my workflow involved, for every project:
1) terminal with CLI agent for most of the coding
2) VScode mostly to look through the code, especially the git tab which gives a convenient view of the diffs, even if I don't personally edit much
3) A few more terminals on tmux for runs/builds/tests

Now that agentic coding has progressed so that you rarely need to write code by hand, I have improved my workflow by ditching VSCode, since is not really needed anymore. This accomplishes 2 things: no need to have a VSCode process open for every project, and all our tools are fully CLI.

Since we're fully CLI, we can put everything into our terminal multiplexer. I use [`herdr`](https://herdr.dev/) which is a better terminal multiplexer than tmux (although both are good), especially for agentic coding.

My `herdr` setup:
1) Use a workspace for every project/repository
2) Use a few tabs for every workspace:
   - CLI agent tab
   - 'main' tab for checking reviewing diffs (with [`hunk`](https://www.hunk.dev/)), git commands, general file stuff
   - some more tabs (possibly with split panels) for runs/builds/tests again, for each of frontend, backend, workers, etc.

Bonus: since we are fully CLI and don't often type code by hand, herdr devs had the smart idea to make their terminal UI mobile-friendly. This means we can connect to our computer with tailscale+`ssh` and work from mobile phone without much inconvenience. So when you `ssh` from your phone, you run `herdr` and it brings up all your open projects with all your tabs instantly, and you can detach anytime and leave them running as they are to reconnect later.