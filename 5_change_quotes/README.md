# Changing surrounding characters

It may seem contrived, but I frequently find the need to change "this" to 'this',
add backticks to a word, or change a list comprehension to a set or generator.
Here are some examples with the desired output.
```
['apple', 'banana', 'cherry']
{"apple", "banana", "cherry"}

[i for i in range(10)]
{i for i in range(10)}

Use :w to write a file
Use `:w` to write a file
```

## Just do it
In the last example, there's nothing wrong with moving your cursor, using ``i` ``
moving it again, and maybe using a `.`.  By the time you come up with something
else, you'd be done with the edit anyways!

## Substitute
For quotes, a good regex to know is
```
:[%]s/'/"/g
```
The optional `%` dictates if you want to change a line or the entire file.
Several ex commands can be chained together with `|`, so the first example could
be solved with
```
s/'/"/g | s/\[/{/g | s/]/}/g
```
In this case, `&` will only replay the last s command.  You can use `@:` instead
to repeat the entire ex command.  Not the simplest for one line, but if you
needed it several times you get some time savings.

## Using registers to change surrounding characters
Finally, we can use yank and delete to replace the surrounding characters.
This uses some tricky details about yank and delete registers, so we will go
slow the first time and focus on `'apple'`:
```
yi'                 yank the contents of the single quotes to register " and 0
ca'                 change the contents of single quotes and the quotes, replace "
"C-r0"              use the yank register to recall in insert mode
```

You can save that to a macro for replay as well.  The same method works for other
braces, but you need a different macro for each.
Macros can be saved into your vimrc so they are always available at startup.
For the bracket example, you can put the following in your vimrc:
```
let @b="yi[ca[{0}"
```
I think this is best used for the last example:
```
f:
ce
`C-r"`
```
Here we can use the unnamed register as there are no existing quotes.


## vim-surround plugin
I won't cover how to install plugins, but when you are ready, the
[vim-surround](https://github.com/tpope/vim-surround) plugin solves this nicely
and plays well with the `.` command. (use `gx` to go to a url in a file)
