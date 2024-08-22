---
title: NVim
subtitle: NVim text editor configuration
date: 2022-10-07
tags: ['computing']
toc: true
draft: false
---

![](/images/vim.jpg)
## Setup
   1. [Install neovim](https://neovim.io/)
   2. [See how to migrate from vim/use the config file](https://neovim.io/doc/user/nvim.html#nvim-from-vim)
   3. [Install plug-in manager](https://github.com/junegunn/vim-plug) 
   4. Install plugins(see script below)
   5. [Learn how to use it efficiently](https://missing.csail.mit.edu/2020/editors/) 

## Config files
### ~/.config/nvim/init.vim

    set runtimepath^=~/.vim runtimepath+=~/.vim/after
    let &packpath = &runtimepath
    source ~/.vimrc

    " Plugin Section
    call plug#begin("~/.vim/plugged")
        Plug 'dracula/vim'
        Plug 'mildewchan/takodachi.vim'
        Plug 'xero/sourcerer.vim'
        Plug 'bluz71/vim-nightfly-guicolors'
        Plug 'sainnhe/sonokai'
        Plug 'lewis6991/moonlight.vim'
        Plug 'arzg/vim-colors-xcode'
        Plug 'mhartington/oceanic-next'
        Plug 'sonph/onehalf', { 'rtp': 'vim' }
        Plug 'tomasr/molokai'
        Plug 'gosukiwi/vim-atom-dark'
        Plug 'tomasiser/vim-code-dark'
        Plug 'joshdick/onedark.vim'
        Plug 'nikolvs/vim-sunbather'
        Plug 'sheerun/vim-polyglot'
        Plug 'vim-airline/vim-airline'
        Plug 'jiangmiao/auto-pairs'
        Plug 'mg979/vim-visual-multi', {'branch': 'master'}
    call plug#end()

    " Config Section
    if (has("termguicolors"))
        set termguicolors
    endif
    syntax enable

    colorscheme onedark
    " Some other color schemes
    " colorscheme dracula
    " colorscheme sourcerer
    " colorscheme takodachi
    " colorscheme nightfly
    " colorscheme sonokai
    " colorscheme moonlight
    " colorscheme xcodedark
    " colorscheme OceanicNext
    " colorscheme onehalfdark
    " colorscheme molokai
    " colorscheme atom-dark
    " colorscheme codedark
    " colorscheme sunbather

### ~/.vimrc

    filetype plugin indent on
    syntax enable
    set autoindent
    set backspace=indent,eol,start
    set complete-=i

    set smarttab
    set tabstop=4
    set shiftwidth=4
    set expandtab
    set wrap

    set titlestring=%t
    set title

    set ttimeout
    set ttimeoutlen=100

    set incsearch
    set ignorecase
    set number
    set laststatus=2
    set ruler
    set showcmd
    set wildmenu
    set showmatch

    set history=500

    set scrolloff=1
    set sidescrolloff=5
    set display+=lastline

    set autoread
    set fileformats+=mac

    if &encoding ==# 'latin1' && has('gui_running')
    set encoding=utf-8
    endif

    if &listchars ==# 'eol:$'
    set listchars=tab:>\ ,trail:-,extends:>,precedes:<,nbsp:+
    endif
    if has('path_extra')
    setglobal tags-=./tags tags^=./tags;
    endif
    inoremap <C-U> <C-G>u<C-U>
    set mouse=a
    command Wq wq
    command WQ wq
    command W w
    command Q q
    nnoremap Q <nop>
    set pastetoggle=<F2>
    set clipboard=unnamedplus
