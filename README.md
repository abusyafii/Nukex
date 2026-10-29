# Nukex

A small Ubuntu CLI that removes software completely: packages from apt, snap, and flatpak, unused dependencies, stale configs, and leftover data in system and home directories.

## Features

- Remove apt, snap, and flatpak packages in one command
- Purge configuration files by default
- Find and delete leftover files under `/etc`, `/var`, `/opt`, `/usr/local`, and your home directory
- Detects apt packages left behind in `config-files` state
- Cleans package caches, unused dependencies, stale configs, and disabled snap revisions
- Safety checks for essential, required, protected, and running-kernel packages
- Dry-run mode, confirmation prompts, and command logging

## Requirements

- Ubuntu or a Debian-based system
- Bash 4+
- `apt` and `dpkg`
- Optional: `snap`, `flatpak`
- `sudo` when not running as root

## Installation

```bash
git clone https://github.com/abusyafii/Nukex.git
cd Nukex
chmod +x nukex
mkdir -p ~/.local/bin
ln -sf "$PWD/nukex" ~/.local/bin/nukex
```

Make sure `~/.local/bin` is in your `PATH`. Run it directly if you prefer:

```bash
./nukex --help
```

## Usage

```bash
nukex <command> [options] [packages...]
```

### Commands

| Command | Description |
| --- | --- |
| `remove`, `rm`, `purge` | Remove packages and optionally their leftovers |
| `leftovers` | Show leftover files, or stale apt configs when no package is given |
| `clean` | Autoremove, clear caches, purge stale configs, remove old snaps |
| `list` | List installed apt, snap, and flatpak packages |
| `search` | Search available packages |

### Options

| Option | Description |
| --- | --- |
| `-n`, `--dry-run` | Show what would happen without changing anything |
| `-y`, `--yes` | Skip the confirmation prompt |
| `-m`, `--manager <name>` | Limit to `apt`, `snap`, `flatpak`, or `auto` (default) |
| `--leftovers` | Also find and remove leftover files |
| `--no-home` | Skip home directory leftovers |
| `--keep-config` | Keep apt/snap configuration and user data |
| `--no-autoremove` | Do not remove unused dependencies |
| `--force` | Allow removing protected, non-essential packages |
| `-h`, `--help` | Show help |

### Examples

```bash
# Remove an app from every package manager and delete its leftovers
nukex rm --leftovers firefox

# Preview a removal without changing anything
nukex rm -n --leftovers code

# Remove only the snap version
nukex rm -m snap spotify

# Show stale apt configs and leftover paths
nukex leftovers

# Delete leftover files for a package
nukex leftovers --remove myapp

# Preview a full cleanup
nukex clean -n

# Search and list packages
nukex search cowsay
nukex list -m snap
```

## Safety

- Essential and `Priority: required` apt packages are always refused.
- Core system packages are refused unless `--force` is used.
- Packages belonging to the running kernel are skipped.
- Every destructive run asks for confirmation unless `-y` is passed.
- `--dry-run` is available for every command.
- System paths are modified with `sudo`; home paths stay owned by your user.

`nukex` deletes files with `rm`; it does not securely overwrite disk blocks. Use a dedicated secure-erase tool if you need that guarantee.

## Logging

Actions are logged to:

```text
~/.local/state/nukex/nukex.log
```
