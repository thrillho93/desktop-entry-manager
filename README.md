# manage_apps

A bash script for managing the GNOME application menu. It combines two
utilities: an alphabetizer for the app grid and folders, and a tool to
disable unwanted desktop entries.

## Features

### Alphabetize
- Sorts apps and folders together in the main GNOME app grid
- Sorts apps within each custom folder
- Sorts the folder list itself
- Merges duplicate folders (same name, different IDs) before sorting
- Skips category-based folders (automatically managed by GNOME)

### Disable Apps
- Hide unwanted desktop entries (`.desktop` files) from the app menu
- Multiple selection modes:
  - **Hardcoded list** — ships with a default set of rarely-needed system entries
  - **Interactive** — pick apps to hide using `fzf`
  - **Config file** — save a selection and reuse it (e.g. after updates)
- Restore all hidden apps with one command
- List currently hidden apps

## Requirements

- `python3` — required (used for GVariant parsing)
- `fzf` — optional, required only for interactive selection mode
- `gsettings` — standard on all GNOME desktops

## Installation

```bash
# Download
curl -o ~/.local/bin/manage_apps \
  https://raw.githubusercontent.com/thrillho93/manage_apps/master/manage_apps
chmod +x ~/.local/bin/manage_apps
```

Or clone and symlink:

```bash
git clone https://github.com/thrillho93/manage_apps.git
ln -s "$PWD/manage_apps/manage_apps" ~/.local/bin/manage_apps
```

## Usage

```bash
manage_apps
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

### Alphabetize

Selecting option 1 will:
1. Clean up any duplicate folders
2. Sort the folder list alphabetically
3. Sort the main app grid (apps and folders interleaved by display name)
4. Sort apps within each custom folder

A GNOME Shell restart is needed to see the changes:
- **Wayland**: log out and log back in
- **X11**: press `Alt+F2`, type `r`, press Enter

### Disable Apps

Selecting option 2 opens a submenu with these choices:

| Option | Description |
|--------|-------------|
| Hardcoded list | Hides a built-in set of rarely-used system entries |
| Interactive (fzf) | Pick apps to hide from a searchable list |
| Interactive + save | Same as above, but saves your selection to a config file |
| Use saved config | Re-apply a previously saved selection |
| List hidden apps | Show all currently hidden entries |
| Restore all | Unhide everything |

Hiding works by renaming `.desktop` files to `.desktop.bak`. Restoring
renames them back. This requires `sudo`.

### Default hidden apps (hardcoded list)

```
avahi-discover, bssh, bvnc, qv4l2, qvidcap,
electron37, uuctl, xgps, xgpsspeed, nm-connection-editor
```

These can be changed by editing the `DESKTOP_FILES` array in the script.
