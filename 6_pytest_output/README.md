# Adding expected output in pytest

Depending on the intensity of a calculation, it's nice to add some acceptance
test output to pytest source files (usually in `conftest.py`).  I had to do
this a lot for some refactoring recently that required converting tsv to csv
and making the file contents behave properly in pytest.  You can also use
block strings, but I think this is an informative example.

Given the file `output.tsv`, we want to populate the StringIO object:
```python
@pytest.fixture
def accepted_output():
    return StringIO(
        'here,is,a,sample\n'
        'here,is,another,sample\n'
    )
```

## Copying in the result
We can get the raw data easily in two ways

### Yank and put
```
Start by opening the output file (with :e, :vs, gf, etc)
ggyG        yank all contents
C-o         jump back
p           put
```
Open, yank, put is easy to remember and if you only want a few lines from the
file it can be handy to pick and choose.  If you have a giant input file or
other constraint, you should move to read.

### Read entire file contents
```
:r output.tsv   copy contents of output.tsv below the cursor
```
Just that easy (except I usually forget the command is called `:r[ead]`).
You can do some other tricks with read we will cover later

## Edits to make
Now that we have the contents we need to do 4 things:
 - convert tabs to commas
 - add a single quote to the start of the line
 - add a newline and quote to the end of the line
 - indent properly

### Convert tabs to commas

#### gn
The `gn` motion is hard to grasp.  Used alone, you search for the next pattern
match and visually highlight it.  Used as part of a command, you act over the
next pattern match.  The interesting thing is you can repeat the move and change
with a single `.`.  Try searching for `/comma` and press enter.  Use `gn` to
visually select the next match.  Next, try `gUgn` to make comma uppercase.

With this in mind, we can search and replace tabs for commas like so
```
/\t<cr>         search for a tab
cgn,<esc>       replace next match with a comma
....            repeat as needed
```

I wouldn't do that in practice, but `gn` may be handy some day!

#### substitute
We are going to cover a few neat things with substitute that will come up again.

First, if you visually select a region and type `:`, the command line is populated
with the current visual range.  So if you run a substitute command it will only
affect those lines.

With substitute, you can add flags to alter its function.  `g` is used frequently
to act over the entire line but `c` is also handy to confirm a match before
performing the replacement.

Overall we have
```
vi(             select in parenthesis
:s/\t/,/gc      substitute each tab for comma and ask first
```
The overall result is the same as `gn` but you are able to examine the result
*before* running the command instead of afterwards.  That can be handy over
multiple lines but in this case I would probably skip the `c` flag altogether.

### Add quotes and newlines
#### substitute
Let's keep moving with substitute.  In addition to literals, you can also target
anchors with substitute to append or prepend to a line.  We can even try to do both in
a single bound, but the range for the second command has to be manually entered:
```
:'<,'>s/^/'/ | '<,'>s/$/\\n'/
```
A more complex substitute command works on the entire line:
```
:s/.*/'\0\n'/
```
Here we match the entire line (`.*`) then use `\0` to recall the contents.  This
prevents the need to specify the range as vim automatically fills in when you
press `:` from visual mode.

#### Manual to macro
Instead of substitute, we can use `I'<esc>` and `A\n'<esc>` to add in the characters.
Clearly this is best for a single line and you can `j.` for a handful.  Let's
also cover the macro since it brings up a few concepts.  First, record the
macro on a single line:
```
qqI'<esc>A\n'<esc>q
```
We have placed the prepend and append commands into the register `q`.  Notice
however that if we repeat the macro, it continues to work on the current line;
using `22@@` would not produce our desired result!  Instead, we have to apply
`@@` once per line that we specify.  That's easy with the `:norm[al]` command,
which allows you to specify a range and a normal command, like `@q`.
```
vi(
:norm @q
```
`:norm` is much more powerful.  The command can be anything and the range could
include a regex.  Say you want to add a semicolon to each line in a c function:
```
vi{
:norm A;
```

The alternative to `:norm` is to change our macro to advance to the next line
with `j`.  Before you re-record `q`, you can instead just add to a macro:
```
qQjq
```
By specifying the uppercase register, you tell vim to add what you're about to
type to register `q`.  Also handy is the `:registers` command, which tells you
the current content of your registers.  There are methods to edit the internals
of a macro, but you may be better suited by switching to another method or
breaking your edits into smaller macros played together with `:norm` for instance.

#### Block Edit
One problem with the other solutions is if the lines are indented you have to
do more work to get what you want.  Not much, but I find block edit to be my
preferred solution unless I know I'm about to do this same edit 10 times.

You enter block edit mode with `C-v`.  In addition to using `c` and `r` as covered
in the table exercise, you can insert to the left with `I` and to the left with
`A`.  If you use `$` to select the end of a line, this even works with ragged edges.
```
0C-v        start block visual mode at the start of the line
select lines to the bottom
I'<esc>     insert single quote
gv$         select same region and move to the end of the line
A\n'<exc>   add end quote
```

### Indent the result
This should be as easy as `=i(`, but python formatting can cause issues with
vim out of the box.  You can try various plugins, setup something like black
with `gq`, or if you're in a hurry, use `>i(...`.  `>` and `<` are commands that
indent and dedent a line by one tabstop.  While we are on the topic of tabs,
note that you can also indent in insert mode with `C-t` and reverse with `C-d`.
Definite life savers in python!

## Exercise: Keyword arguments to a dict
Here's another problem you may encounter with unit testing.  Say you have an
object that you can set with keyword arguments but want to compare to a dict.
If you don't know python, we basically want to make:
```
to_test = MyThing(a=1, b="2", c=[])
```
and use the thing in parentheses to make this in curly braces:
```
assert to_test.__dict__ == {
        'a': 1,
        'b': "2",
        'c': [],
    }
```

Take a few minutes to try below:
```
to_test = MyThing(a=1, b="2", c=[])
assert to_test.__dict__ ==
```
