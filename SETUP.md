# Multi-Repo Sync - Quick Setup Guide

## Quick Setup for Your Team

### 1. Initial Setup

```bash
# Make the script executable (if not already)
chmod +x /path/to/multi-repo-sync

# Or let the installer do it:
/path/to/multi-repo-sync --install
```

### 2. Configure Repositories

```bash
multi-repo-sync --configure
```

This will:
- Ask for the base directory where your repositories are located
- Allow you to add all your repositories
- Let you enable/disable specific repositories
- Show which repositories are active/inactive

### 3. Create an Alias (Optional but Recommended)

```bash
multi-repo-sync --install
```

The installer will:
- Automatically make the script executable
- Ask for the desired alias name (default: `mrsync`)
- Add it to your shell configuration (~/.bashrc, ~/.zshrc, or ~/.bash_profile)

You can use any name you prefer:
- `mrsync` (default and recommended)
- `sync`
- `repo-sync`
- etc.

### 4. Start Using

```bash
# View current branches
mrsync status

# Switch branches
mrsync develop
mrsync feature/888

# Switch with dependency installation
mrsync develop -d

# View configuration
mrsync --show-config

# Get help anytime
mrsync --help
```

## Common Commands

```bash
# Check status (without making changes)
mrsync status
mrsync --status

# Switch to a branch (with pattern matching)
mrsync develop              # Exact match
mrsync feature/888          # Finds feature/888-* branches
mrsync fix/123 -d           # With dependencies

# Reconfigure later
mrsync --configure          # Update configuration
mrsync --show-config        # View current configuration

# Share with others
# Just send the script file and run the configuration
```

## Configuration File

After initial configuration, edit `~/.multi-repo-sync.conf` to:
- Add custom repositories
- Enable/disable specific repositories
- Change repository paths
- Modify synchronization order

Example:
```bash
# Add a custom repository
REPO_PATHS["my-custom-lib"]="/path/to/custom/lib"
REPO_ENABLED["my-custom-lib"]=true
REPO_ORDER=("my-custom-lib" "backend" "frontend")
```

## Permissions

The `--install` command automatically sets the script as executable:
```bash
chmod +x /path/to/multi-repo-sync
```

If needed, you can do this manually before installation.

## Notes

- Configuration is stored in `~/.multi-repo-sync.conf` (one per user)
- Each team member can have different paths and aliases
- The script works with any Git repository structure
- Custom repositories can be added with `--configure`

## Features

- Intelligent branch pattern matching (finds branches like `feature/888-*`)
- Optional dependency installation
- Show current status without making changes
- Support for custom repositories
- Colorized output
- Configurable aliases
- Works with bash, zsh, and bash_profile
- Parallel processing for speed
- Ambiguous branch detection
- Detailed error messages

## Version Management

Check the current version:
```bash
mrsync --version
```

The tool uses Git tags for version tracking. If you're working from a clone, you'll see the latest tag version. If you're working from a development version, you'll see a commit hash.

---

Created by poses
