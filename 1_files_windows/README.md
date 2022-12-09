# Files and Windows
Dealing with multiple files is commonplace during development; you need to add
a declaration to a header then add the implementation to the source.  You
write a test then the method, you look at some example input and output
while writing your code.  Vim has two related, yet distinct views of files:
buffers and windows.

## Opening and Saving Files
When you open a file and start editing it, those changes are kept in memory in
a buffer.  Only once you save those changes is the new version written to disk.
You can also open a new file and the original file is kept in a buffer (by default).
Let's start with commands for saving:
```
:w[rite]            write the file
:wa[ll]             write all open buffers
:wqa                write all open buffers, exit vim
:up[date]           write if the buffer has changed
:w <FILE>           write this buffer to a new filename, original is open
:sav[eas] <FILE>    write this buffer to a new filename, new file is open
```
I always use write but if you have a giant file it may be more suitable to
use update.  Saveas is almost always what you want over the write variant.

Now to open files:
```
:e                  reopen this file, useful to sync with disk
:e!                 reopen this file, discard any buffer changes
:e <FILE>           open the file in this window, original buffer is open but hidden
:find <FILE>        find a file in path and edit it

:pwd                current working directory
:cd <DIR>           change current directory
gf                  go to file under cursor (relative to wd)
```
So you can open `subdir/file1.txt` from this directory by any of the following:
```
gf
:e subdir/file1.txt
:find s*/*1.txt
```
Find must resolve to a single file.  Since there is a `file1.txt` in the current
directory and subdirectory, we have to specify the `s*/` part.  To get to
`subdir/sub.txt`, using `:find sub.txt` is sufficient.

After you open a file you can jump back with `C-o` and forward with `C-i`.
To list all open buffers, you can use `:ls` but typically I tend to ignore
open buffers.

## Multiple Windows
A window in vim allows you to have a different view of a buffer.
```
:sp                 horizontal split
C-ws                horizontal split
:vs                 vertical split
C-wv                vertical split
C-wo                close all other windows
```
Splits can be moved and resized as well.  Without any other arguments,
the split contains the same active buffer, but you can also specify a filename
to `:sp` or `:vs` to open a new file in a split.  Opening the same buffer is
handy for large files or when you need to look at an implementation while using
it.

A set of windows are contained in a tab.  You can work with tabs with ex commands:
```
:tabe <FILE>         open a new tab with file or an empty buffer
:tabe %              open a new tab with current file
:tabc                close this tab
:tabo                close all other tabs
gt,gT               cycle through tabs
```
The buffer, window, tab model of vim is slightly different than an IDE.  A
tab is a collection of windows.  A window is a view on a buffer. A buffer is
an in-memory view of a file.  Once you have a window you can change it to any
buffer.
