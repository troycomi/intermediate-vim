# Closing Thoughts

I hope you've learned a few tricks to start expanding your vim repertoire.
You won't use everything right away, but pick your favorite one or two and see
if you can make it muscle memory. **Come back, pick another, and repeat**.

To get better at anything, you have to have the curiosity to seek out new ideas
or methods and the willingness to practice and improve.  If you spend most of
your time editing text, you already have a perfect environment to practice.
You were curious enough to sign up for a workshop, so all you need is to give
yourself permission to work slower while you try to learn something new.  You
may be slower today, but will be so much faster in a few weeks!

## Additional resources
 - I'm a huge fan of [vimcasts](vimcasts.org) for bite-sized topics you can
   start using today.
 - Drew Neil also has published two books, "Practical vim" and "Modern vim".
   This workshop was heavily influenced by Practical vim.
 - "Learning the vi and vim Editors" by Robbins, Hannah, and Lamb is more
   of a reference book but contains tons of useful information.
 - There are a surprising number of youtube channels that discuss vim as well.

## Vim clones
This workshop was purposefully meant to cover aspects of vim that will map to
any variant of vim, including vim plugins in IDEs.  There are also several
vim-like editors with various strengths.  I prefer neovim, but once you know
modal editing the only sticking points will be how and how much you need to
customize your experience.

## Plugins
Because of the range of vim clones, it's hard to discuss plugins without getting
into specific plugin managers and systems.  You don't need most plugins, but
that doesn't mean you don't want them!  Once you are ready to work with plugins,
I recommend most everything Tim Pope makes.  Here are the ones in my current
vimrc:
 - [tpope/vim-commentary](https://github.com/tpope/vim-commentary)
    Easily add or remove comments from any source code
 - [tpope/vim-fugitive](https://github.com/tpope/vim-fugitive)
    Nice git integration, a life saver for merge conflicts
 - [tpope/vim-surround](https://github.com/tpope/vim-surround)
    Adds operations to add, change, or delete surrounding characters
 - [tpope/vim-unimpaired](https://github.com/tpope/vim-unimpaired)
    Adds complementary keymaps like `[ ` to add a line or `[e` to exchange
 - [tpope/vim-repeat](https://github.com/tpope/vim-repeat)
    Improves the repeat command to work with more plugins
 - [tpope/vim-eunuch](https://github.com/tpope/vim-eunuch)
    Adds several helpers to perform operations like moving files.  Automatically
    chmods files that start with a shebang.
