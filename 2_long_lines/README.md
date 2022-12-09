# Wrapping long lines

If you are coming from an IDE (or collaborating with someone) frequently the
automatic line wrapping leads to lines of code that go on forever:
```
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

How do we make the output something that is easy to read in the terminal and
github?
```
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
culpa qui officia deserunt mollit anim id est laborum.
```

## Duplicating Lines
To start, we will need the same text several times.  Here are a few ways to
do this:
 - move, copy, put
 - copy into register
 - `:t`  (see also `:m` for moving lines)

Also set `:let &colorcolumn=80` to get an indication of where you are

## Manually
Move to the 80th character with `80|`, move to the previous space with `F `,
then replace the space with a newline `r<CR>`.

To undo this operation, you can join the current line with the one below using `J`.

## Macro
Macros in vim are powerful in their simplicity.  You can record a series of
keystrokes with `q` then replay them at any time with `@`.  Each macro
is saved into a register (the same registers as yank) so 26 are available.
Let's record the previous command into a macro:
```
qq80|F r<CR>q
```
and replay the contents of the `q` register with `@q`.  You can replay the
last macro with `@@` and even include a count `22@q`.

## Substitute
The substitute command in vim is really a joy to work with because it's almost
interactive sed.  Most operations you do can be converted to a clever substitute
command.  The trade off is that crafting that command can take a while!

We want to match the first 80 characters then add a newline, try:
```
:s/^\(.\{1,80}\) /\1^M/
```
Since we break the line, adding a global flag won't work.  You can repeat the
last `:s` with `[#]&`.  If you want to apply the substitute to all lines, you
can use `g&`.  The result should match the macro

## gq
I was legitimately upset when I learned about the `gq` command because I had
been using the macro approach for a while!
If you have `textwidth` set, `gq` will automatically format the line for you.
You can even set a `formatprg` to run an external formatter instead!
```
gql             format this line
gggqG           format entire file
gqip            format this block 
```
Sometimes the output will mangle new lines, but for markdown files `gq` is a
great time saver!
