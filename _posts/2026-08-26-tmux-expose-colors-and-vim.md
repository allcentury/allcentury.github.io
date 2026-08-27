---
layout: post
title: Making tmux.expose match my setup
---

Not every PR I've sent to [tmux.expose](https://github.com/cesarferreira/tmux.expose) is about AI agents, I've [got a post coming](/2026/08/27/tmux-expose-agent-status/) on agent-aware tmux sessions too. This post is about adding vim navigation + custom colors to tmux.expose.

## Configurable colors

tmux.expose highlighted the selected, attached, and inactive cards in hardcoded yellow, green, and white. Those are fine colors, just not *my* colors, everything else in my setup (this blog, nvim, my terminal) is Dracula themed, and tmux.expose was the one thing that stuck out every time I popped it open.

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

Because typing any letter normally fuzzy-filters the list, hjkl can't both move the cursor and be filter text at the same time, so vim mode is modal same as vim itself. NORMAL mode moves with hjkl or arrows, `/` enters search, Enter switches, q or Esc quits.

That was the whole story for a couple of months, until living with it daily turned up a gap: typing `/demo` then hitting `Esc` cleared the search outright and dropped you back into the full, unfiltered list every time. There was no way to filter down to a handful of sessions and then just browse that subset with hjkl, you had to pick a match immediately or start over.  Small bug that I [patched here](https://github.com/cesarferreira/tmux.expose/pull/7)

![tmux.expose vim navigation demo](/public/imgs/tmux-expose-vim-nav.gif)

I love making small improvements to my tools - it feels like my day to day keeps improving.
