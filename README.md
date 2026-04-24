# clipboard.yazi

Synchronize files between the Yazi file manager and your system clipboard. Supports both copy and paste on macOS and Linux desktops.

## Features

- **Copy**: Yank files in Yazi and make them available to other applications via the system clipboard.
- **Paste**: Copy files from the system clipboard into the current Yazi directory, with conflict resolution (overwrite / rename / skip).

## Requirements

| Platform | Copy | Paste |
| -------- | ---- | ----- |
| Linux (X11) | `xclip` | `xclip` |
| Linux (Wayland) | `wl-copy` (`wl-clipboard`) | `wl-paste` (`wl-clipboard`) |
| macOS | Built-in `osascript` | Built-in `osascript` |

## Installation

Install the plugin via the package manager:

```bash
ya pkg add XYenon/clipboard
```

This clones the repository, adds it to `~/.config/yazi/package.toml`, and pins the current revision.

## Usage

Add shortcuts in `~/.config/yazi/keymap.toml`:

```toml
# Copy yanked files to the system clipboard
[[mgr.prepend_keymap]]
on  = "y"
run = [ "yank", 'plugin clipboard -- --action=copy' ]

# Paste files from the system clipboard into the current directory
[[mgr.prepend_keymap]]
on  = "<C-p>"
run = [ 'plugin clipboard -- --action=paste' ]
```

## Optional arguments

The plugin accepts the boolean argument `notify-unknown-display-server`:

- Default `false`: silently exit when the Linux display server is unknown (useful for TTY or remote sessions).
- `true`: show a notification to warn that the operation is unavailable in the current session.

Example invocation:

```toml
[[mgr.prepend_keymap]]
on  = "y"
run = [ "yank", 'plugin clipboard -- --action=copy --notify-unknown-display-server' ]
```

## Troubleshooting

- **`Copy failed: xclip/wl-copy not found`**: install `xclip` for X11 or `wl-clipboard` (`wl-copy`) for Wayland.
- **`Paste failed: xclip/wl-paste not found`**: install `xclip` for X11 or `wl-clipboard` (`wl-paste`) for Wayland.
- **`Unknown display server`**: ensure Yazi runs in a Wayland or X11 session. Enable `notify-unknown-display-server` to surface a visible warning.

## Development

This repository uses [treefmt](https://github.com/numtide/treefmt) for formatting:

```bash
nix fmt
```

Feel free to open a PR to support more desktop environments.
