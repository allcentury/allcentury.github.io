---
layout: post
title: Making tmux.expose agent-aware
---

Somewhere between the last two posts on my tmux setup, I picked up [tmux.expose](https://github.com/cesarferreira/tmux.expose), a Mission Control style session switcher, live terminal previews of every session in a grid, jump to the one you want. It pairs nicely with `mux`, `mux` gets each project into its own session, tmux.expose is how I actually find the one I want among a dozen running at once.

It's also a Rust TUI, and I don't write a ton of Rust day to day, so contributing to it has been a good excuse to spend real time in a language I'm less fluent in than Ruby or Typescript. I've sent a few small PRs over, configurable highlight colors, a vim navigation mode, a filter input bug fix, but the one worth writing about is the one I opened this week.

## The problem

My pane layout from the [last post](/2026/08/15/tmuxinator-templates/) has a Claude Code pane in it, and that's not unusual for me anymore, most of my sessions have an agent running in one pane or another. Which is great until you have six or seven sessions going and tmux.expose is showing all of them in the same flat, alphabetical grid regardless of what's actually happening inside them. One agent might be three minutes into a task. Another finished and is sitting there waiting on me. Another hit a permission prompt and has been stuck for twenty minutes because I haven't looked at that pane. The grid doesn't know the difference, so I don't either, until I manually click through everything.

That's a bad way to find out an agent has been blocked on you for twenty minutes.

Here's the fix running against a few demo sessions, watch the left card flip to `needs you` and the right one catch up behind it:

![tmux.expose sessions re-sorting as agent status changes](/public/imgs/tmux-expose-agents.gif)

## The fix

I opened [a PR](https://github.com/cesarferreira/tmux.expose/pull/6) that makes the grid agent-aware. The mechanism is two tmux pane options:

| tmux pane option | Meaning |
|---|---|
| `@agent_status` | `working`, `waiting`, or `attention` |
| `@agent_status_since` | Unix timestamp of when that status started |

The moment something writes those on a pane, tmux.expose picks it up on the next refresh. No config required to make it display, it works on any agent that can run a shell command on a state change, not just Claude Code. A session's card takes the *worst* status across all of its panes (`attention` > `waiting` > `working`), so a multi-agent session never hides a stuck pane behind a busy one, and by default sessions with a waiting or blocked agent sort to the top of the grid, oldest-waiting first. That last part matters more than it sounds like it should, the agent that's been waiting the longest is usually the one I forgot about.

The binary ships a one-shot helper so nothing but `tmux-expose` itself needs to be on `PATH`:

```bash
tmux-expose agent-status working    # agent just started a turn
tmux-expose agent-status waiting    # agent's turn ended, idle on you
tmux-expose agent-status attention  # agent is blocked (e.g. needs a permission)
tmux-expose agent-status clear      # agent/session is done, remove the marker
```

Wiring it into Claude Code is just hooks in `~/.claude/settings.json`:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      { "hooks": [{ "type": "command", "command": "tmux-expose agent-status working" }] }
    ],
    "Stop": [
      { "hooks": [{ "type": "command", "command": "tmux-expose agent-status waiting" }] }
    ],
    "Notification": [
      {
        "matcher": "permission_prompt",
        "hooks": [{ "type": "command", "command": "tmux-expose agent-status attention" }]
      }
    ],
    "SessionEnd": [
      {
        "matcher": "",
        "hooks": [{ "type": "command", "command": "tmux-expose agent-status clear" }]
      }
    ]
  }
}
```

That's it, no separate script to write or distribute, just four hooks pointed at a binary that's already on `PATH`.

## Multi-agent sessions

The gif above is two single-pane demo sessions, but a session with more than one agent pane spells out its worst status in words and shows the rest as compact counts, `‼ needs you · ⏳2 · ⚙1`. A single tracked pane just gets the plain label, `⏳ awaiting reply`. I went back and forth on using bare emoji for all of it, but landed on words for the status that actually matters, emoji rendering varies by terminal font, and a symbol that silently fails to render is a bad failure mode for the one status you actually need to see. Here's the full grid:

![tmux.expose agent status](https://github.com/user-attachments/assets/eb1408d5-f868-4a09-bdce-0de87a1f36ce)

The PR's still open as I'm writing this, waiting on review, but I've been running it locally for a few days and I've stopped tabbing through sessions to find the one that's stuck. That's the whole point of contributing to a tool you actually use every day, you're not guessing what would be useful, you already know because you were just annoyed by its absence an hour ago.
