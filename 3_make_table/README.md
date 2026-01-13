# Tables and Boarders

I don't think you should really use these tips frequently, but they introduce
a couple of visual block features that could be broadly useful.

## Introducing visual mode

Visual mode is a way to select text in a way that might feel more similar
to how you would edit text in a word processor.  To enter visual mode, press
`v` in normal mode.  You can then move the cursor around and text will be
selected. You can then use `d` to delete the selected text, `y` to yank it,
or `c` to change it, just like in normal mode. 

> __Exercise: visual-mode-basics__:
>
> Enter visual mode to highlight THIS word (vaw) then delete it
>
> Enter visual mode and select from HERE to the end of the line (v$) and yank it
> then paste the yanked text below this line (p)


## Introducing visual line mode

Visual line mode is similar to visual mode, but selects whole lines at a time.
That's pretty much it. You can enter visual line mode with `V` in normal mode.

> __Exercise: visual-line-mode__:
>
> Enter visual line mode and select all 3 lines.
> Yank the selected lines. Without moving your cursor,
> paste them once, what happened? Undo and try P instead of p.


> __Exercise: swap-case__:
>
> oH NO! tHE UPPER/LOWER CASE. iT IS COMPLETELY SWAPPED!
>
> Fix the above line by entering visual line mode and selecting
> the line, then pressing `~` to swap the case of each character in the line.

## Introducing visual block mode

Visual block mode is similar to visual mode, but selects a rectangular block of text.
You can enter visual block mode with `C-v` in normal mode.
You can then move the cursor around to select text.
You can even use `o` to take control of the opposite corner of the block.

    Exercise: visual-block-columns:

    Enter visual block mode and highlight the entire grid below.
    Next try highlighting just the 2nd column.
    Finally try deleting the 3rd column with `x`.

    12345
    12345
    12345
    12345

## TSV to a text table
Given a tab-separated text file, how do we draw a nice table?  To start, we will
use `data.tsv` in this directory and try to make the following table:
```
---------------------------------------------------------------------
NAME          |  COURSE  |  ITEM         |   COMMENT                |
---------------------------------------------------------------------
Troy          |  Main    |  Lasagna      |                          |
Steve         |  Side    |  Greek salad  |   Not vegan!             |
Barbara Anne  |  Drinks  |  Punch        |   A really long comment  |
---------------------------------------------------------------------
```
Here are the main steps, read entirety before trying!
```
:r data.tsv         read in the file
:retab              convert tabs to spaces
align columns
capitalize header with VgU
select ends of lines with block visual then add spaces to end of each line (40A<space>)
block replace separator between columns
yypVr-              add in horizontal ruler
Block copy and space to adjust spacing, try using o and O to move corners
```
Try it yourself on the line below and we will go over a solution.


## Boarders
Personally I dislike having decoration in source code as it just wastes disk
and screen space.  However, I did always wonder how they were made...

```
80i*<esc>
yy4p
C-v       visual block select interior
r<space>
R<new words>
```

Try it out here:


Here are the steps for centered entries:
```
Write out the rows you want
visual-line select and :center
pad to 80 characters 
make top and bottom rows
make column markers
```

It can be helpful to use `:let &colorcolumn=80` to guide your work

Try centering the following 3 lines to replicate the table below

Whole seasons are spent
Mastering the form, the style.
None calls it easy.


Here's what the centered header could look like:
```
--------------------------------------------------------------------------------
|                           Whole seasons are spent                            |
|                        Mastering the form, the style.                        |
|                             None calls it easy.                              |
--------------------------------------------------------------------------------
```
