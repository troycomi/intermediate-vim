# Tables and Boarders

I don't think you should really use these tips frequently, but they introduce
a couple of visual block features that could be broadly useful.

## TSV to a text table
Given a tab-separated text file, how to we draw a nice table?  To start, we will
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
Here are the main steps:
```
:r data.tsv         read in the file
:retab              convert tabs to spaces
align columns
capitalize header with VgU
add spaces to end of each line (40a<space>)
block replace separator between columns
yypVr-              add in horizontal ruler
Block copy and space to adjust spacing, try using o and O to move corners
```

## Boarders
Personally I dislike having decoration in source code as it just wastes disk
and screen space.  However, I did always wonder how they were made...

```
80i*<esc>
yy4p
V       visual block select interior
r<space>
R<new words>
```
Here are the steps for centered entries:
```
Write out the rows you want
:center
pad to 80 characters
make top and bottom rows
make columns
```
