# Incrementing Numbers

Here is another problem that starts contrived but I use frequently in pytest.

## Adding numbers to a bulleted list
Here are a list of some Marvel films
```
 - Iron Man
 - The Incredible Hulk
 - Iron Man 2
 - Thor
 - Captain America: The First Avenger
 - The Avengers
 - Iron Man 3
 - Thor: The Dark World
 - Captain America: The Winter Soldier
 - Guardians of the Galaxy
 - Avengers: Age of Ultron
```

Let's change them to a numbered list:
```
 1) Iron Man
 2) The Incredible Hulk
 3) Iron Man 2
 4) Thor
 5) Captain America: The First Avenger
 6) The Avengers
 7) Iron Man 3
 8) Thor: The Dark World
 9) Captain America: The Winter Soldier
 10) Guardians of the Galaxy
 11) Avengers: Age of Ultron
```

Here are the steps
```
C-v     select bullet points
c1)     Replace with 1)
gv      Reselect bloc visual
move to the second entry
gC-a    increment as a counter
```
Try it!

Of note with `C-a`, the cursor will move to the first number on the line. `C-x`
decrements and you can add a count to do some simple math (2+3 -> 2 `3C-a`).
If you need more math operations, you can use `C-r=` to evaluate simple operations
and vim expressions.

## Copy with increment
When writing unit tests, I will sometimes have to assert the value of a list
of values, such as:
```python
assert line[0] == "blah"
# ...
assert line[10] == "blat"
```

### Put once, increment once
A nice macro for the job is `qqyypC-aq`, that is yank, put and increment.
The value of this approach is it's easy to make X copies by running `X@q`.
If each line builds on the last, you alternate `@q`, edit, repeat.  The downside
is it only increments the first number in each line.
You can add it to your `.vimrc`:
```vim
let @i='yyp'  " the missing character is C-a.  To add in, use C-vC-a.
```
so `@i` always increments.

### Put many times, increment once
This is similar to the numbered list example.  You yank the line, use `#p` to
make however many copies you want, then block select the numbers and `gC-a`.
The advantage is it doesn't occupy a register and you can increment anywhere
in the line.  Things are more challenging if each line builds on the last.

Try one method to fill in the missing assert lines.
