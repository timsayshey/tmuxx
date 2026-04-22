# tmuxx

A small tmux wrapper that hides numeric suffixes and collapses grouped sessions,
so you see one logical name per session instead of a list of near-duplicates.

## Why

tmux lets multiple sessions share the same set of windows via *session groups*.
Some terminal setups (iTerm integration, attach scripts, etc.) create a new
grouped session every time you attach, giving you lists that look like:

```
backend-5
backend-27
backend-28
wp-eiq-37
wp-eiq-38
wp-eiq-39
wp-eiq-40
wp-eiq-41
```

All of those `wp-eiq-*` rows are the same underlying workspace — just with
independent focus. `tmuxx` collapses them to one name per group and lets you
attach/create/kill by that name.

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
tmuxx new <name>      create a new session
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
