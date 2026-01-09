# Claude Code Plugin - Git Worktree Manager

A Claude Code plugin that provides easy git worktree management. Create and remove git worktrees with simple commands.

## Features

- **Create Worktrees**: Easily create new git worktrees in a standardized directory structure
- **Auto-Navigation**: Automatically navigate to newly created worktrees
- **Remove Worktrees**: Safely remove worktrees with built-in safety checks
- **Error Handling**: Comprehensive validation and error messages

## Prerequisites

- Claude Code version 1.0.33 or later
- Git version 2.5 or later (for worktree support)
- A git repository

## Installation

### From GitHub (Recommended)

```bash
/plugin install https://github.com/KouheiOkazaki/claude-code-plugin
```

### Local Development

For testing or development, you can load the plugin locally:

```bash
claude --plugin-dir ./claude-code-plugin
```

## Configuration

### Required Permissions

Add the following permissions to your `.claude/settings.local.json` to allow the plugin to execute git commands:

```json
{
  "permissions": {
    "allow": [
      "Bash(git worktree:*)",
      "Bash(mkdir:*)",
      "Bash(rm:*)",
      "Bash(cd:*)",
      "Bash(pwd:*)",
      "Bash(git rev-parse:*)"
    ]
  }
}
```

## Usage

### Creating a Worktree

Create a new git worktree with a specified name:

```
/claude-code-plugin:add-git-worktree <worktree-name>
```

**Example:**

```
/claude-code-plugin:add-git-worktree feature-auth
```

This will:
1. Create a directory at `../worktrees/feature-auth`
2. Create a new branch named `feature-auth`
3. Set up the worktree in that directory
4. Automatically navigate to the new worktree

**Directory Structure:**

```
your-repo/               # Your main repository
├── .git/
├── src/
└── ...

worktrees/               # Created automatically (sibling to your repo)
└── feature-auth/        # Your new worktree
    ├── .git
    ├── src/
    └── ...
```

### Removing a Worktree

Remove an existing git worktree:

```
/claude-code-plugin:remove-worktree <worktree-name>
```

**Example:**

```
/claude-code-plugin:remove-worktree feature-auth
```

This will:
1. Verify the worktree exists
2. Check for uncommitted changes (warns if found)
3. Remove the worktree from git's tracking
4. Clean up the directory

### Safety Features

The plugin includes several safety features:

- **Duplicate Detection**: Prevents creating worktrees with duplicate names
- **Uncommitted Changes Warning**: Warns before removing worktrees with uncommitted changes
- **Repository Validation**: Ensures you're in a git repository before operations
- **Path Verification**: Verifies worktree paths exist before removal

## Commands Reference

| Command | Description | Arguments |
|---------|-------------|-----------|
| `/claude-code-plugin:add-git-worktree` | Create a new worktree | `<worktree-name>` |
| `/claude-code-plugin:remove-worktree` | Remove an existing worktree | `<worktree-name>` |

## Worktree Naming Convention

Worktrees are created using the following path pattern:

```
../worktrees/<worktree-name>
```

This means:
- Worktrees are created in a `worktrees/` directory
- The `worktrees/` directory is a sibling to your main repository directory
- Each worktree gets its own subdirectory named after the branch

**Example:**

If your repository is at `/Users/you/projects/my-app`, worktrees will be created at:
- `/Users/you/projects/worktrees/feature-1`
- `/Users/you/projects/worktrees/feature-2`
- etc.

## Troubleshooting

### "Not in a git repository" error

Make sure you're running the command from within a git repository. You can verify with:

```bash
git status
```

### "Worktree already exists" error

A worktree with that name already exists. You can:
1. Choose a different name
2. Remove the existing worktree first using `/claude-code-plugin:remove-worktree`
3. List existing worktrees with: `git worktree list`

### "Branch name already exists" error

A branch with that name already exists. You can:
1. Choose a different name
2. Checkout the existing branch in a new worktree location (the plugin will offer this option)

### Permission errors

If you encounter permission errors, ensure you've added the required permissions to your `.claude/settings.local.json` file (see Configuration section above).

### Git version issues

Git worktrees require Git 2.5 or later. Check your version:

```bash
git --version
```

## Examples

### Example 1: Creating a feature branch worktree

```
User: /claude-code-plugin:add-git-worktree feature-login

Claude: I'll create a new worktree for feature-login...

✓ Created worktree at ../worktrees/feature-login
✓ Created branch feature-login
✓ Navigated to ../worktrees/feature-login

You can now start working on your feature!
```

### Example 2: Removing a completed feature worktree

```
User: /claude-code-plugin:remove-worktree feature-login

Claude: I'll remove the worktree feature-login...

✓ Verified worktree exists
✓ Checked for uncommitted changes (none found)
✓ Removed worktree ../worktrees/feature-login
✓ Cleaned up directory

Worktree feature-login has been successfully removed.
```

### Example 3: Handling uncommitted changes

```
User: /claude-code-plugin:remove-worktree feature-wip

Claude: Warning: The worktree feature-wip has uncommitted changes.

Are you sure you want to remove it? This will discard all uncommitted work.
- Type 'yes' to force remove
- Type 'no' to cancel
```

## How It Works

### Add Worktree Workflow

1. Validates the worktree name is provided
2. Checks if you're in a git repository
3. Verifies the worktree doesn't already exist
4. Creates `../worktrees/` directory if needed
5. Executes `git worktree add ../worktrees/<name> -b <name>`
6. Navigates to the new worktree directory
7. Confirms successful creation

### Remove Worktree Workflow

1. Validates the worktree name is provided
2. Checks if you're in a git repository
3. Verifies the worktree exists
4. Checks for uncommitted changes
5. Executes `git worktree remove ../worktrees/<name>` (with `--force` if needed)
6. Cleans up any remaining directories
7. Confirms successful removal

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

MIT License - See LICENSE file for details

## Support

If you encounter any issues or have questions:
1. Check the Troubleshooting section above
2. Review your permissions in `.claude/settings.local.json`
3. Ensure you're using Git 2.5+ and Claude Code 1.0.33+
4. Open an issue on GitHub: https://github.com/k-okazaki/claude-code-plugin/issues

## Version History

### 1.0.0 (Initial Release)
- Add git worktree creation with auto-navigation
- Remove git worktree with safety checks
- Comprehensive error handling and validation
