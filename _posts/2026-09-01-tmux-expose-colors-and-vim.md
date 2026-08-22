---
layout: post
title: Making tmux.expose match my setup
published: false
---

Not every PR I've sent to [tmux.expose](https://github.com/cesarferreira/tmux.expose) is about AI agents. Before I got to [agent-aware sorting](/2026/08/24/tmux-expose-agent-status/), I sent over two smaller ones that are really about the same thing: making a tool I use all day stop looking and feeling like a default install.

## Configurable colors

tmux.expose highlighted the selected, attached, and inactive cards in hardcoded yellow, green, and white. Fine colors, just not *my* colors, everything else in my setup (this blog, nvim, my terminal) is Dracula, and tmux.expose was the one thing that stuck out every time I popped it open.

[The PR](https://github.com/cesarferreira/tmux.expose/pull/2) exposes all three as tmux options and matching CLI flags, values accept color names, 256-color indices, or hex:

```tmux
set -g @tmux-expose-selected-color '#bd93f9'
set -g @tmux-expose-attached-color '#50fa7b'
set -g @tmux-expose-inactive-color '#6272a4'
```

Defaults preserve the old yellow/green/white behavior, so this is opt-in, nobody's grid changes colors out from under them on upgrade.

Before (defaults):

![tmux.expose with default highlight colors](/public/imgs/tmux-expose-default-colors.png)

After (mine):

![tmux.expose with Dracula highlight colors](https://github.com/user-attachments/assets/575c11a6-261e-4064-ab7e-cdd0cc15bb33)

## Vim navigation

I've been customizing vim keybindings since [before this blog covered anything else](/2016/11/21/vim-timeout/), hjkl is closer to muscle memory for me than arrow keys at this point. tmux.expose's picker is type-to-filter by default though, which means hjkl is just filter text unless you opt out of that.

[The PR](https://github.com/cesarferreira/tmux.expose/pull/5) adds an opt-in modal vim mode:

```tmux
set -g @tmux-expose-vim-keys 'on'
```

Because typing any letter normally fuzzy-filters the list, hjkl can't both move the cursor and be filter text at the same time, so vim mode is modal same as vim itself. NORMAL mode moves with hjkl or arrows, `/` enters search, Enter switches, q or Esc quits. SEARCH mode is the old behavior, typing filters, Esc drops back to NORMAL. The footer hints change depending on which mode you're in so it's never ambiguous which one you're in.

Here it is in NORMAL mode, hjkl moving the selection, `/` dropping into search, `Esc` back out:

![tmux.expose vim navigation demo](/public/imgs/tmux-expose-vim-nav.gif)

Neither of these PRs are exciting the way agent-aware sorting is, they're just the kind of thing you notice needs fixing after using something daily for a few weeks, and then you're already in the codebase so you fix it.
