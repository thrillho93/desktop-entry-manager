# desktop_entry_manager

A bash script for managing `.desktop` entry files on Linux. It combines two
utilities: a GNOME app grid alphabetizer, and a universal tool to hide unwanted
application launcher entries.

## Features

### Alphabetize *(GNOME only)*
- Sorts apps and folders together in the GNOME app grid
- Sorts apps within each custom folder
- Sorts the folder list itself
- Merges duplicate folders (same name, different IDs) before sorting
- Skips category-based folders (automatically managed by GNOME)

### Disable / Hide Desktop Entries *(any XDG desktop)*
Works on any desktop environment that reads `.desktop` files — GNOME, KDE,
XFCE, Cinnamon, etc.

- Hide unwanted entries from application launchers
- Multiple selection modes:
  - **Hardcoded list** — ships with a default set of rarely-needed system entries
  - **Interactive** — pick entries to hide using `fzf`
  - **Config file** — save a selection and reuse it (e.g. after package updates restore entries)
- Restore all hidden entries with one command
- List currently hidden entries

## Requirements

- `python3` — required (used for GVariant parsing in the GNOME alphabetize feature)
- `gsettings` — required for the alphabetize feature (standard on GNOME desktops)
- `fzf` — optional, required only for interactive selection mode

## Installation

```bash
# Download
curl -o ~/.local/bin/desktop_entry_manager \
  https://raw.githubusercontent.com/thrillho93/desktop_entry_manager/master/manage_apps
chmod +x ~/.local/bin/desktop_entry_manager
```

Or clone and symlink:

```bash
git clone https://github.com/thrillho93/desktop_entry_manager.git
ln -s "$PWD/desktop_entry_manager/manage_apps" ~/.local/bin/desktop_entry_manager
```

## Usage

```bash
desktop_entry_manager
```

A menu will appear:

```
╔════════════════════════════════════════════════════╗
║                   Apps Manager                     ║
╚════════════════════════════════════════════════════╝

Choose an action:
  1) Alphabetize GNOME apps and folders
  2) Disable/hide desktop applications
  3) Exit
```

### Alphabetize *(GNOME only)*

Selecting option 1 will:
1. Clean up any duplicate folders
2. Sort the folder list alphabetically
3. Sort the main app grid (apps and folders interleaved by display name)
4. Sort apps within each custom folder

A GNOME Shell restart is needed to see the changes:
- **Wayland**: log out and log back in
- **X11**: press `Alt+F2`, type `r`, press Enter

### Disable / Hide Entries *(any XDG desktop)*

Selecting option 2 opens a submenu:

| Option | Description |
|--------|-------------|
| Hardcoded list | Hides a built-in set of rarely-used system entries |
| Interactive (fzf) | Pick entries to hide from a searchable list |
| Interactive + save | Same as above, but saves your selection to a config file |
| Use saved config | Re-apply a previously saved selection |
| List hidden entries | Show all currently hidden entries |
| Restore all | Unhide everything |

Hiding works by renaming `.desktop` files to `.desktop.bak`. Restoring
renames them back. Requires `sudo` since entries live in `/usr/share/applications`.

### Default hidden entries (hardcoded list)

```
avahi-discover, bssh, bvnc, qv4l2, qvidcap,
electron37, uuctl, xgps, xgpsspeed, nm-connection-editor
```

These can be changed by editing the `DESKTOP_FILES` array in the script.
