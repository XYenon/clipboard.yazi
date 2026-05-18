# clipboard.yazi

Synchronize files between the Yazi file manager and your system clipboard. Supports both copy and paste on Linux, macOS, and Windows.

## Features

- **Copy**: Yank files in Yazi and make them available to other applications via the system clipboard.
- **Paste**: Copy files from the system clipboard into the current Yazi directory, with conflict resolution (overwrite / rename / skip).

## Requirements

| Platform | Copy | Paste |
| --- | --- | --- |
| Linux (X11) | `xclip` | `xclip` |
| Linux (Wayland) | `wl-copy` (`wl-clipboard`) | `wl-paste` (`wl-clipboard`) |
| macOS | Built-in `osascript` | Built-in `osascript` |
| Windows | Built-in `powershell` | Built-in `powershell` |

Windows support uses `powershell.exe` (Windows PowerShell 5.1), which ships with Windows 10/11 and requires no additional installation. Files are placed on the clipboard as a `CF_HDROP` file drop list, the same format used by Explorer, so yanked files can be pasted directly into Explorer, VS Code, and other apps with Ctrl+V.

## Installation

Install the plugin via the package manager:

```
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
- **Windows: `Copy failed: powershell failed`**: ensure `powershell.exe` is on your `PATH`. It ships at `%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe` on all Windows 10/11 installations. If you are running Yazi inside WSL, the Linux code paths apply instead.
- **Windows: pasted files appear as copies, not moves**: `CF_HDROP` carries copy semantics by default. Cut-and-move is not currently supported on Windows (the same limitation applies to the macOS backend).

## Development

This repository uses [treefmt](https://github.com/numtide/treefmt) for formatting:

```
nix fmt
```

Feel free to open a PR to support more desktop environments.
