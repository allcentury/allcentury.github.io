---
layout: post
title: Why your Vim hotkeys might be slow...
---

I have been using [ctrlp](https://github.com/kien/ctrlp.vim) with [ag](https://github.com/ggreer/the_silver_searcher) as my file fuzzy finder for well over 2 years now.  It has been fantastic - it's accurate, easy to navigate and mostly importantly fast.

Whenever I open vim, I instantly hit `<leader> f` to bring up a file list. Here's a quick screenshot:

![ctrlp](/public/imgs/vim-timeout-ctrlp-filelist.png)

Using the fuzzy finder:

![ctrlp2](/public/imgs/vim-timeout-ctrlp-search.png)

Great - that is a joy to code with.  If you see [my post below](/2016/11/17/vim-javascript-es6/) on my vim shortcut to replace `function`'s with ES6 fat arrows, I ran into something with my vim setup that was unsuspected.

Prior to that post, I had my fuzzy finder mapped to `<leader>f`, as shown here in my `~/.vimrc`:

```
" set leader key to comma
let mapleader = ","

" silver searcher config
let g:ackprg = 'ag --nogroup --nocolor --column'

" ctrlp config
let g:ctrlp_map = '<leader>f'
let g:ctrlp_max_height = 30
let g:ctrlp_working_path_mode = 0
let g:ctrlp_match_window_reversed = 0
let g:ctrlp_show_hidden = 1
```

Then, as my post below mentions I added a new mapping `<leader>fa` (fa meaning 'fat arrow').  In my `.vimrc` that looked like:

```
" replace function(*args) with fat arrows =>
map <leader>fa :%s/(\(.*\)function\(.*)\)/(\1\2 =>/g<cr>
```

I've been using both of those hotkeys just fine over the last few days.  However, one thing I did notice is that my vim began to lag when I wanted to search files.  I'd hit `,f` and it would hang for a second then display the file list.  I'm so used to that being very fast that I was miss hitting files or worse leaving the prompt and entering text.

After some googling around I learned about a vim setting that I've never had to tweak before called timeout.

In vim, go to `:h timeout`. The first sentence describes exactly what I was running into:

>[timeout and ttimeout] - These two options together determine the behavior when part of a mapped key sequence or keyboard code has been received

Most importantly it states in the next paragraph that vim waits 1 second for another character to arrive before it will default to the lesser mapping.  So in my case I never had another hotkey mapped to `f` before but now that I have `f` and `fa` vim is waiting to see that once I hit `f` will I also hit `a`?

You're probably wondering if there is a way to turn off the `timeout` and sure enough there is:

```
:set notimeout
:set nottimeout
```

I suggest going through `:map` and seeing if you could benefit from adjusting your timeout based on your own key mappings.  Now you need to figure out what you're going to do with all those free seconds I just gave you back - hopefully you're in the giving spirit as we are just a few weeks away from [code.org's hour of code](https://hourofcode.com/us).  I volunteered last year and it was a blast, I hope you do the same!




