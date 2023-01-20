# Vim grammar, the dot command, and a basic .vimrc

## Why can't you just be normal (mode)?!

One of the most foreign concepts for beginner vim users is its modal nature.  In
every other editor you use, when you type a character the letter appears.  Some
hotkeys are available with control or alt, but mostly you move with the mouse
or arrows and then you type.

In vim, what a character does depends on the context, and most importantly, the
mode.  The most important modes are:
 - normal: how vim starts and allows for navigation and sending commands
 - insert: typing
 - visual: highlighting regions to edit
 - command: writing ex commands

Think of normal mode as the central hub of your editing.  From normal mode you
can move to any other mode and back.  Use normal mode whenever you are thinking
or complete an edit.

## Vim command language

In normal mode, every letter is a shortcut to an operation.  Some move the
cursor, some make changes, some move to different modes.  Learning more about a
key is as easy as typing `:h <KEY>`.  As a quick reminder we have the following
movement keys
```
h,j,k,l     move cursor by one space
0,^,$       move to beginning or end of a line
w,e,b       move by a word
t,f         move forward to a character
n,;,,       move to a search or character match
```
Shift with each of these produces a similar movement
```
W,E,B       move by a WORD
T,F         move backward to a character
N           move to a search backwards
```
Moves can also be much larger
```
}               move to the next blank line
gg,G            move to start,end of file
<#>g            move to line #
C-f,C-u         move down,up by a screen
C-e,C-y         move down,up one line
C-o,C-i         jump backward,forward
m<c>,`<c>       mark location as `c`, recall it
%               move between matching (,[,{,",'
z[z,-,enter]    move window so curser is in the middle, bottom, top
```

Moving between modes is also a keypress away:
```
i,a,o,etc   normal  ->  insert
v           normal  ->  visual
:           normal  ->  command
ESC         ANY     ->  normal
```

Because you frequently need to move from normal to insert mode, there are
multiple variants to accelerate some common edits:
```
i           insert "behind" cursor
I           insert at begining of line
a           insert "ahead of" cursor
A           insert at end of line
o           insert as a newline below cursor
O           insert as a newline above cursor
```

And most of the rest edit text, these are called operators
```
r           replace
s           substitute one character, like cl
x           delete one character
u           undo, VERY handy
d           delete
c           change; go to insert mode
y           yank
p           paste (or put)
```

Each operator can be combined with a movement to act exactly where you need,
for example, here are some delete variants:
```
dl          delete character to the left (same as x)
dh          delete character to the right
dw          delete word
df.         delete everything including the first .
dt.         delete everything up to the first .
d$          delete to end of line
```

Most operators have some common shortcuts:
```
dd          delete line
D           delete to end of line (like d$)
yy          yank line
Y           yank to end of line
```

Each motion can be combined with a count, for example:
```
d3w         delete 3 words
3dw         delete 3 words
3yy         yank 3 lines
3p          put the register contents 3 times
```

Instead of a motion, you can use a search command or text objects.  Text objects
are represented by `i` or `a` and the surrounding object
```
daw             delete around a word
ciw             change in a word
ci"             change in "double quotes"
ca"             change around "double quotes"
c/cat<enter>    change to the first instance of "cat"
```
You don't even need to be on the object, just in front of it or on the same line!
So `ci"` can replace `f"lct"`.  As a bonus, text objects work better with the dot
command.

Some meta-commands are just handy enough they become muscle memory:
```
ea              append to end of word
xp              transpose two characters
```

### Escape the escape key!

After about 2 minutes you should question why the escape key is used to move
from insert to normal mode, it's all the way up THERE!  The reason is keyboard
layouts have changed since vi(m) was invented and software engineers are stubborn.
You have a couple of options:
 - get a different keyboard
 - remap `jk` or `kj` to escape
 - use `C-[`
 - remap escape to caps lock

Until I wrote this section, I was using the caps lock as escape.  Since, I
mapped caps lock to control then use `C-[`.  It took a little practice, but is
much nicer for all the other commands you use control for (`C-t` in chrome,
`C-w` to delete the last word in insert mode, etc). You'd be surprised how
little you miss having a caps lock option!

## The dot command

The dot command will replay the last operation you performed.  That may seem like
a silly thing to do, but using the dot command effectively will drastically improve
your efficiency.  Often times in development, you need to perform the same edit
throughout a file or function.  There are 3 parts to effective dot usage:

### Make edits in a repeatable way

Using `ci"` is better than `f"lct"` because the latter will only get repeated
to `ct"`; motions are discarded.  If you spend long stretches in insert mode,
when you go to undo or repeat you will have large, chunky changes.  When you
need to delete 7 words, it may be faster to use `dw......` than having to count
the number of words to delete.

### Work, dot, u

Get in the habit of making small changes, repeating as needed with dot, and undoing
if you go too far.  It uses more key strokes but it uses much less mental load.

### Rapid fire dot

A common workflow is to move to a position then repeat a change.  This could be
searching with regex, moving to a character, or changing lines.  The motion to
achieve is `n.n.n.`, `;.;.;.`, or `j.j.j.`, move and repeat (undo if needed).
If you can make your commands follow one of these patterns, you are close to
optimal.

### Don't dot too much

As you are firing off `j.j.j.`, at some point, you will wonder, should I have
used something different?  At this point the answer is yes!  But you can't change
the past and you then need to decide if you should change the approach now or
just finish your rapid `j.`.  As you get better at gauging your work and can
craft operations faster the breakpoint for using a macro or regex will shift.
My cutoff is around 10 repetitions; just be aware that there are always better
ways, but they may be slower the first time you have to look it up!

## Your new .vimrc

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

set number relativenumber   "Display line numbers
set nowrap                  "Do not wrap long lines
nnoremap <Up> <Nop>
nnoremap <Down> <Nop>
nnoremap <Right> <Nop>
nnoremap <Left> <Nop>
```
Some additions may be recommended but this will be a good start.  Relative
numbering is a great way to assist with multiline commands and jumps.

If you want to improve your usage of `#j` and other movement commands, I highly
recommend slowing key repeat and increasing repeat delay in your OS!

### Adding common abbreviations
Another handy collection of commands to add to your vimrc are are abbreviations.
In insert mode, you can use abbreviations as substitutions for long phrases,
common misspellings, or frequent code constructs.  Note that if you want to use
abbreviations for something like a function declaration or a for loop, you should
look into a snippet plugin.  Snippets behave like abbreviations, but allow you
to fill in parts of the expanded text, e.g. function name, arguments, etc.

Here are some example abbreviations for your vimrc:
```vim
abbreviate teh the  " common misspelling
" long or common phrases
abbreviate PUaddr Princeton University. Princeton, NJ 08544
abbreviate const public static final int
```
