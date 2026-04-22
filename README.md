# tmuxx

A small tmux wrapper that hides numeric suffixes and collapses grouped sessions,
so you see one logical name per session instead of a list of near-duplicates.

## Why

I run a lot of parallel Claude Code sessions in tmux — one per project or
experiment. Two things got annoying:

1. `tmux attach -t <name>`, `tmux new -s <name>`, `tmux kill-session -t <name>`
   is a lot of typing for something I do dozens of times a day.
2. tmux lets multiple sessions share the same windows via *session groups*.
   If you're moving fast and run `tmux new -s frontend` when a `frontend`
   session already exists, tmux (or whatever wrapper your terminal uses)
   quietly creates a grouped duplicate with an auto-appended number —
   `frontend-38`, `frontend-39`, and so on. Some setups (iTerm's tmux
   integration, attach helpers) do the same thing on `attach`, which is
   more annoying because you weren't even trying to make a new session.
   Either way the list grows into something like:

   ```
   backend-5
   backend-27
   backend-28
   frontend-37
   frontend-38
   frontend-39
   frontend-40
   frontend-41
   ```

   Every `frontend-*` row is the same underlying workspace with independent
   focus. Finding the one you actually want to attach to is noise.

`tmuxx` collapses each group down to one logical name and gives you short
verbs — `tmuxx`, `tmuxx frontend`, `tmuxx new api`, `tmuxx kill frontend`.
`kill` removes every member of the group, so you don't have to clean up
stragglers by hand.

## Install

Drop `tmuxx` somewhere on your `PATH` and make it executable:

```sh
curl -o ~/.local/bin/tmuxx https://raw.githubusercontent.com/timsayshey/tmuxx/main/tmuxx
chmod +x ~/.local/bin/tmuxx
```

Requires `tmux` and `bash`.

## Usage

```
tmuxx                 list sessions (one per group)
tmuxx <name>          attach to session
tmuxx new <name>      create a new session (attaches if it already exists)
tmuxx kill <name>     kill a session (all members of its group)
tmuxx -h | help       show help
```

`tmuxx <name>` resolves `<name>` to the attached session in that group, or the
most recent one if none are attached. `tmuxx kill` removes every session in
the group, not just one member.

If you're already inside tmux, `tmuxx <name>` and `tmuxx new <name>` use
`switch-client` instead of `attach-session`.

## License

MIT. See `LICENSE`.
