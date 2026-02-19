# Multi-Repo Sync

A powerful bash script for synchronizing branch changes across multiple Git repositories with parallel processing and intelligent dependency management.

## Features

- 🚀 **Parallel Processing** - Updates multiple repositories simultaneously for maximum speed
- 🎯 **Smart Branch Matching** - Intelligent pattern matching (e.g., `feature/888` finds `feature/888-*`)
- 🔍 **Ambiguous Branch Detection** - Alerts when multiple branches match a pattern
- 📦 **Optional Dependency Installation** - Choose whether to install dependencies during sync
- 🎨 **Colorized Output** - Clear, easy-to-read status messages
- ⚙️ **Configurable** - Customize repository paths and sync order
- 🔧 **Flexible Alias System** - Create your own command alias

## Quick Start

### Installation

1. **Download the script**:

   ```bash
   git clone <repository-url>
   cd multi-repo-sync
   ```

2. **Configure your repositories**:

   ```bash
   ./multi-repo-sync --configure
   ```

3. **Create an alias** (optional but recommended):

   ```bash
   ./multi-repo-sync --install
   # Default alias: mrsync
   ```

4. **Reload your shell**:

   ```bash
   source ~/.bashrc  # or ~/.zshrc
   ```

### Basic Usage

```bash
# Check current status
mrsync status

# Switch branches (without installing dependencies)
mrsync develop
mrsync feature/888

# Switch branches and install dependencies
mrsync develop -d
mrsync feature/888 --dependencies

# Preview changes without making them
mrsync feature/888 --dry-run

# View configuration
mrsync --show-config

# Show version
mrsync --version

# Get help
mrsync --help
```

## Commands

### Branch Synchronization

```bash
mrsync <branch-pattern> [options]
```

**Options:**

- `-d, --dependencies` - Install/update dependencies after switching branches
- `--dry-run` - Preview what would happen without making changes
- `-b, --branch <pattern>` - Explicitly specify branch pattern

**Examples:**

```bash
mrsync develop              # Switch to develop branch
mrsync feature/888          # Switch to feature/888* branches
mrsync fix/123 -d           # Switch and install dependencies
mrsync --dry-run develop    # Preview what would happen
```

### Status Commands

```bash
mrsync status               # Show current branch for all repos
mrsync --show-config        # Display current configuration
mrsync --version            # Show version information
```

### Configuration Commands

```bash
mrsync --configure          # Configure repository paths
mrsync --install            # Create command alias
mrsync --help               # Show help message
```

## Configuration

After running `--configure`, the configuration is stored in `~/.multi-repo-sync.conf`.

### Configuration File Format

```bash
# Repository paths (key=name, value=absolute path)
REPO_PATHS["frontend"]="$HOME/projects/my-frontend"
REPO_PATHS["backend"]="$HOME/projects/my-backend"
REPO_PATHS["shared-lib"]="$HOME/projects/shared-library"

# Enable/disable repositories (true/false)
REPO_ENABLED["frontend"]=true
REPO_ENABLED["backend"]=true
REPO_ENABLED["shared-lib"]=true

# Order of synchronization (space-separated list)
REPO_ORDER=("shared-lib" "backend" "frontend")
```

### Customization

You can edit `~/.multi-repo-sync.conf` to:

- Add or remove repositories
- Enable/disable specific repositories
- Change the synchronization order
- Update repository paths

## How It Works

### Default Behavior (without `-d`)

1. Validates all repository paths
2. Fetches latest changes from remote
3. Searches for branches matching the pattern
4. Checks out matching branches (in parallel)
5. Pulls latest changes
6. Displays final status

### With Dependencies (`-d` flag)

After the sync process, the script will:

- Install dependencies based on project type detected:
  - **Python projects**: `pip install -r requirements.txt`
  - **Node.js projects**: `npm install`
  - **Library dependencies**: `pip install -e .`

### Branch Pattern Matching

The script uses intelligent pattern matching:

- `develop` - Exact match for "develop"
- `feature/888` - Matches "feature/888", "feature/888-\*", etc.
- `fix/123-urgent` - Exact match or partial match

If multiple branches match, the script will display all options and stop.

## Handling Uncommitted Changes

When uncommitted changes are detected, the script offers three options:

1. **Stash changes** - Automatically stashes changes with a descriptive message
2. **Abort** - Keeps working on current changes without making any modifications
3. **Continue anyway** - Proceeds despite uncommitted changes (may cause checkout to fail)

To restore stashed changes:

```bash
cd <repository>
git stash pop
```

## Performance

Typical execution times for 4 repositories:

- **Without dependencies**: ~2-5 seconds
- **With dependencies**: ~30-60 seconds (depends on cache state)

The parallel processing approach makes syncing multiple repositories significantly faster than sequential execution.

## Troubleshooting

### Branch Not Found

The script searches for partial matches. To see available branches:

```bash
cd /path/to/repository
git branch -a | grep <pattern>
```

### Multiple Branches Match

Use a more specific pattern:

```bash
# Instead of:
mrsync feature/888

# If multiple matches exist, use full name:
mrsync feature/888-user-management
```

### Dependency Installation Fails

Run manually to see detailed error messages:

```bash
# For Python projects
cd /path/to/project
pip install -r requirements.txt

# For Node.js projects
cd /path/to/project
npm install
```

### Reconfigure Repositories

```bash
mrsync --configure
```

### View Current Configuration

```bash
mrsync --show-config
```

## Sharing with Your Team

1. Share the script file with your team
2. Each team member runs `./multi-repo-sync --configure`
3. Everyone configures their own local repository paths
4. Optionally create aliases: `./multi-repo-sync --install`

Each team member can have different:

- Repository locations
- Enabled/disabled repositories
- Alias names

## Version Control

The script uses Git tags for version management. To check the current version:

```bash
mrsync --version
```

## Requirements

- Bash 4.0 or higher
- Git
- (Optional) Python with pip for Python projects
- (Optional) Node.js with npm for JavaScript projects

## Contributing

Contributions are welcome! This tool is designed to be project-agnostic and work with any Git-based repository structure.

## License

MIT License - See [LICENSE](LICENSE) file for details.

## Author

Created by poses

---

**Note**: This tool is designed to work with any multi-repository project structure. It's completely configurable and doesn't make assumptions about your project organization.
