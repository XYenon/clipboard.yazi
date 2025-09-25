# clipboard.yazi

Synchronize the file paths currently yanked in the Yazi file manager with your system clipboard. The plugin supports macOS and Linux desktops out of the box.

## Requirements

| Platform | Dependency |
| -------- | ---------- |
| Linux | `xclip` (X11), `wl-copy` (Wayland) |
| macOS | Built-in `osascript` |

## Installation

Install the plugin via the package manager:

```bash
ya pkg add XYenon/clipboard
```

This clones the repository, adds it to `~/.config/yazi/package.toml`, and pins the current revision.

## Usage

Add a shortcut in `~/.config/yazi/keymap.toml`:

```toml
[[mgr.prepend_keymap]]
on  = "y"
run = [ "yank", 'plugin clipboard -- --action=copy' ]
```

## Optional arguments

The plugin accepts the boolean argument `notify-unknown-display-server`:

- Default `false`: silently exit when the Linux display server is unknown (useful for TTY or remote sessions).
- `true`: show a notification to warn that copying is unavailable in the current session.

Example invocation:

```toml
[[mgr.prepend_keymap]]
on  = "y"
run = [ "yank", 'plugin clipboard -- --action=copy --notify-unknown-display-server' ]
```

## Troubleshooting

- **`Copy failed: xclip/wl-copy not found`**: install `xclip` for X11 or `wl-clipboard` (`wl-copy`) for Wayland.
- **`Unknown display server`**: ensure Yazi runs in a Wayland or X11 session. Enable `notify-unknown-display-server` to surface a visible warning.

## Development

This repository uses [treefmt](https://github.com/numtide/treefmt) for formatting:

```bash
nix fmt
```

Feel free to open a PR to support more desktop environments or add paste functionality.
