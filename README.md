Open this file in vi then follow along by using "j" to move down.

# Intermediate vim

Vim is one of my favorite tools, which is an odd thing to write about a text
editor.  If you are a developer, you spend most of your time editing text
documents.  If you are a lucky developer, those documents will be consistently
formatted.  Most of your mental energy is spent thinking how to solve a problem
or implement an algorithm.  Most of your *time* is spent copy and pasting, searching
for a declaration, changing camel case variables to snake case, indenting and re-indenting.

When I primarily used an IDE, I would constantly move from keyboard to mouse with my
hand poised over the control-C/control-V (`C-C` and `C-V` from now on).  I could
copy a code block then copy/paste a new variable name or other alteration and get
into a groove of double click, `C-V`.  With vim, you `y`ank and `p`ut, move to
a location with laser precision and hardly ever touch the mouse.  But it gets better,
you can write a macro with a single stroke, `q`, and play it back over 100 lines.
You can craft the perfect substitute command and execute it on every source file
in your project.  On the continuum of expertise, vim let's you quickly pick how
to balance flexibility, repeatability, and robustness to minimize the only metric
that matters, wall-clock time. As the boring parts of editing text take less time,
you spend more time thinking and working on harder problems. The text just flows
and slowly mutates while you envision what to write.

## Beginner and advanced vim

First, I should clarify what we will learn by stating what will not be covered.
This workshop is meant to be a stepping stone towards vim mastery.  I expect you
to know how to navigate in files and perform some modifications.  A common
workflow would be "open file, go to line, use arrow keys to get to a word,
change the word then save and exit".  You should know how to get into and out
of normal and insert mode and can use vim like nano.

At the other end, advanced vim for me means having all the plugins needed to
make your session effectively an IDE and integrate with any other tools you need.
We will not cover this for a few reasons:
 - Plugins add a lot of cruft to vim
 - Plugins are open source, actively developed, and therefore tend to change
 - All of what we will cover is suitable for any vim flavor (neovim or IDE plugins)
 - The best set of tools for your work is highly variable

My best advice, regardless of your level, is to stay away from pre-built configurations
like astrovim, lunarvim, etc.  They do a great job of getting lots of plugins
to play together, but they will overwhelm you with hotkeys and features you
will never use.  You can look them over for inspiration, but really think about
a pain point you have and search for a solution instead of looking at plugins
that seem interesting.

## Getting started

You need to have vim installed.  While there shouldn't be any version-specific
information, we will be using vim, version 8, which is installed on most RC
systems.


# Setting up your new .vimrc

We are going to start with a limited set of vimrc options so we have approximately
the same minimal, environment
```vim
"~/.vimrc
set nocompatible              " be iMproved, required
filetype plugin indent on     " required
set laststatus=2              " Always show the statusline
syntax on                     " syntax highlighting

" highly recommend getting a better colorscheme too!
set background=dark
colorscheme default

set expandtab               "Insert spaces instead of tabs in insert mode. Use spaces for indents
set tabstop=4               "Number of spaces that a <Tab> in the file counts for
set shiftwidth=4            "Number of spaces to use for each step of (auto)indent
set autoindent              "Always set auto-indenting on"

set hlsearch                "highlight search matches
set incsearch               "highlight while typing

set hidden                  "allow modified buffers to hide

set number                  "Display line numbers
"set number relativenumber  "Display line numbers in relative positions
set nowrap                  "Do not wrap long lines
"nnoremap <Up> <Nop>
"nnoremap <Down> <Nop>
"nnoremap <Right> <Nop>
"nnoremap <Left> <Nop>

"Jump to the last pos from previous file opening
au BufReadPost * if line("'\"") > 0 && line("'\"") <= line("$") | exe "normal g'\"" | endif

"Type jk in succession to go from insert to normal mode (to avoid ESC)
":imap jk <Esc>
```

Some additions may be recommended but this will be a good start. Relative numbering is a great way to assist with multiline commands and jumps, but I've turned it off for now.  If you want to improve your usage of `#j` and other movement commands, I highly
recommend slowing key repeat and increasing repeat delay in your OS!


To make this your vimrc, first back up your current one with:

```mv ~/.vimrc ~/.vimrc.bak```

then copy the included vimrc to `~/.vimrc` with:

```cp class_vimrc.txt ~/.vimrc```

If you are using a different OS, you may have a different location for your vimrc, but it should be in your home directory.

## Contents

After covering some basics, we will work through several example editing tasks
to introduce solutions at several levels.  Often, the fastest way to edit a
single line is not the fastest to edit 10 lines and a different solution is
required for 1000 lines.

