# Vim grammar, the dot command, and a basic .vimrc

## Why can't you just be normal (mode)?!

One of the most foreign concepts for beginner vim users is its modal nature.  In
every other editor you use, when you type a character the letter appears.  Some
hotkeys are available with control or alt, but mostly you move with the mouse
or arrows and then you type.

In vim, what a key-press does depends on the context, and most importantly, the
mode.  The most important modes are:
 - normal: allows for navigation and sending commands.
           This is the initial mode you're dropped into
           when you open a file. We're in it now!
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
*           search for the word under the cursor forwards
```
Shift with each of these produces a similar movement
```
W,E,B       move by a WORD e.g this-is-a-WORD (try w vs. W)
T,F         move backward to a character
N           move to a search backwards
#           search for the word under the cursor backwards
```

> __Exercise: navigate-between-markers__:
> Try putting your cursor here [x] and then move to here [y]

> __Exercise: navigate-between-markers-2__:
> Try to get here [y] starting from here [x]

> __Exercise: multi-line-navigation__:
> What if you started here [x] and
> somehow needed to make your way
> all the way towards the end of this line to get here [y]?

Moves can also be much larger
```
}               move to the next blank line
gg,G            move to start,end of file
<#>g            move to line #
C-f,C-u         move down,up by a screen
C-e,C-y         move screen down,up one line
C-o,C-i         jump backward,forward through cursor history
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

> __Exercise: normal-insert-mode-fix-typos__:
>
>    Fx the typo on this line by going into insert mode
>
>    Fixx the typo on this line using x
>
>    Fix this line by adding the missing lette

Undoing and redoing are very common operations
```
u           undo
C-r         redo
```

> __Exercise: undo-redo__: Undo the previous change you made then redo it

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

> __Exercise: combine-count-and-motion-delete__:
>
> Delete the next 3 words VIM IS LAME to fix this line
>
> This entire line is lame too, you should probably delete it
>
> But this line is great, in fact you should yank and paste it
>
> Starting here [x], see if you can delete this entire line and the next
> two lines with the same command.
> (Hint you can try modifying `dd` or `j`)
> DONT DELETE ME

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

> __Exercise: text-objects-in-quotes__:
>
> Translate the quoted word to english "hola"
>
> This function should NOT take arguments
> def foo(bar, baz):
>     print("Hello World!")

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
throughout a file or function.


> __Exercise: dot-delete-quotes-with-text-objects__:
> The following lines have incorrect historical quotes,
> I'm not sure what the correct quotes are, but as a
> first step lets just delete what's in the quotes
> to avoid any future issues
>
> Abe Lincoln - "Don't trust everything you see on the internet"
> Ruth Bader Ginsburg - "The executive and legaslative branches are the best"
> Shia LaBeouf - "It's time to stop pursuing your dreams"



There are 3 parts to effective dot usage:

### Make edits in a repeatable way

Using `ci"` is better than `f"lct"` because the latter will only get repeated
to `ct"`; motions are discarded.  If you spend long stretches in insert mode,
when you go to undo or repeat you will have large, chunky changes.  When you
need to delete 7 words, it may be faster to use `dw......` than having to count
the number of words to delete.

### Work, dot, u

Get in the habit of making small changes, repeating as needed with dot, and undoing
if you go too far.  It uses more key strokes but it uses much less mental load.

> __Exercise: dot-append-trademarks__:
> Each of the Trademarked items indicated by * should have a (Tm) added to the end
>
> Oreos*
> Earth
> Gatorade*
> Powerade*
> Wind
> Fire
> Earth Wind and Fire*

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

> __Exercise: compare-x-vs-de-for-undoability__:
>
> Try removing the extraaaaaa using x a few times and then undoing it
>
> Now try removing the extra using de and then undoing it

> __Exercise: dot-delete-move-skip-words__:
>     DeleTe every worD on This liNe that haS A capital leTter
>     (Hint: repeat dw with . and skip words with w)

### Adding common abbreviations to your .vimrc
Another handy collection of commands to add to your vimrc are are abbreviations.
In insert mode, you can use abbreviations as substitutions for long phrases,
common misspellings, or frequent code constructs.  Note that if you want to use
abbreviations for something like a function declaration or a for loop, you should
look into a snippet plugin.  Snippets behave like abbreviations, but allow you
to fill in parts of the expanded text, e.g. function name, arguments, etc.

Here are some example abbreviations for your vimrc:
```vim
" long or common phrases (this is in the class .vimrc)
abbreviate PUaddr Princeton University. Princeton, NJ 08544

" common misspelling
abbreviate teh the
```

### Extra moving and editing exercises

Here are some additional exercises that you can use to practice the movement 
and operator commands introduced above.

> __Exercise: percent-match-brackets__:
>
> python_dict = {
>     'a': 1,
>     'b': 3,
> } #delete this comment using % to move between brackets


> __Exercise: star-search-and-delete__:
> `my_var` is accidentally being overwritten on the third line below.
> Position your cursor over `my_var` on the first line then use `*`
> to search for the next occurrence of `my_var`.
> Delete the line that overwrites `my_var` using `dd`.
>
>     my_var = 17
>     other_var = 3
>     my_var = 'oops I shouldnt be overwriting this'

> __Exercise: yank-paste-dot__:
> This line and the next line are separated by an empty line
>
> It's true, there's an empty line above.
>
> It would be nice if all the other lines also had a separating line
> that would make the lines easier to read
> and it shouldn't be too hard to do by yanking the empty line
> pasting it where required and then moving down to the next line
> and finally using the dot command `.` to repeat it as much
> as is necessary to get it all done

> __Exercise: break-long-line-with-dot__:
> Sometimes I'll have a shell command in a `.sh` file that is
> really long like this one:
>
> samtools view -f 3 -F 3584 -L chr1.bed -o filtered_reads.sam -U removed_reads.sam mouse_reads.bam
>
> but it would be nicer to read if it were broken up into multiple lines like this:
>
> samtools view \
>   -f 3 \
>   -F 3584 \
>   -L chr1.bed \
>   -o filtered_reads.sam \
>   -U removed_reads.sam \
>   mouse_reads.bam
>
> I would do this with a combination of "forward to space"
> followed by typing the \, going to the next line with enter
> then going back to normal mode, and using `;` to repeat the movement
> twice, then `.` to repeat the next line, indenting etc.
> might need to do a final indenting
>
> after the first line, this becomes `;;.;;.;;.`
