# Files and Windows
Dealing with multiple files is commonplace during development; you need to add
a declaration to a header then add the implementation to the source.  You
write a test then the method, you look at some example input and output
while writing your code.  Vim has two related, yet distinct views of files:
buffers and windows.

## Opening and Saving Files
When you open a file and start editing it, those changes are kept in memory in
a buffer.  Only once you save those changes is the new version written to disk.
You can also open a new file and the original file is kept in a buffer 
(by default). Let's start with commands for saving:
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
:find <FILE>        find a file in path and edit it (works with tab completion)

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
directory and subdirectory, we have to specify the `s*/` part.

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
C-ww                toggle between windows
C-wl                move to the window on the right
C-wh                move to the window on the left
C-wk                move to the window up
C-wj                move to the window down
```
Splits can be moved and resized as well.  Without any other arguments,
the split contains the same active buffer, but you can also specify a filename
to `:sp` or `:vs` to open a new file in a split.  Opening the same buffer is
handy for large files or when you need to look at an implementation while using
it.

> __Exercise: vertical-split__:
>
>    Make a vertical split of this buffer using :vs
>    then move to that window using `C-wl`
>    then try deleting WORD
>
>    It should update in both windows! Then C-wo to close the vertical split

> __Exercise: horizontal-split-file__:
>
>    Make a horizontal split of file2.txt using :sp file2.txt

A set of windows are contained in a tab.
You can work with tabs with ex commands:
```
:tabe <FILE>         open a new tab with file or an empty buffer
:tabe %              open a new tab with current file
:tabc                close this tab
:tabo                close all other tabs
gt,gT                cycle through tabs
```

> __Exercise: tab-navigation__:
>
>  Make a new tab of file1.txt using `:tabe file1.txt`
>
>  Then follow the directions there, go ahead and do that now.
>
>  After you end up back here use gT to go back one more time!
>
>  Phew, welcome back

The buffer, window, tab model of vim is slightly different than an IDE.  A
tab is a collection of windows.  A window is a view on a buffer. A buffer is
an in-memory view of a file.  Once you have a window you can change it to any
buffer.

## Multiple files and reading from stdin

You can open multiple files at once with vim from the command line using
wildcards such as `vi *.txt`. By default, this does not open each file in a
separate tab, and instead you can move from one file to the next with :n or :N.
You can also use `:tab all` to open each buffer in a separate tab.
Adding the -p flag from the command line, `vi -p *.txt`, will open each
file as a separate tab and saves you the step of having to specify `:tab all`.
Alternatively command-line flags `-o` and `-O` allow you to open the multiple
files as either vertically or horizontally stacked windows.

You can also read from stdin with vim. I find this is useful when you
want to concatenate a bunch of files together and then edit them.  For
example `cat *.txt | vi -`.

## Saving your commands to a file

If you'd like, you can have vim write a file of all the commands you
issued in your last session, by using `vi -w commands.txt file3.txt`.
I haven't used this much, but it allows you to count your keystrokes
if you're into that sort of thing.

Once you `:wq` out of vim, you can use `wc -c commands.txt` to count the
number of characters in the file.  
You can also view the file with `vi commands.txt`

You can also repeat the commands from the file by using 
`vi -s commands.txt other_file.txt.` but I haven't found a good use for this.
