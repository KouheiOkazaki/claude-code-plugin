---
name: remove-worktree
description: Removes an existing git worktree from ../worktrees/<name> directory. Use when the user wants to delete a worktree they no longer need.
---

# Remove Git Worktree Skill

This skill removes an existing git worktree with the specified name.

## Workflow

When the user invokes this skill with a worktree name (e.g., `/git-worktree:remove-worktree feature-branch`):

1. **Validate the input**:
   - Check that the user provided a worktree name in `$ARGUMENTS`
   - If no name is provided, list all existing worktrees and ask which one to remove

2. **Check prerequisites**:
   - Verify we're in a git repository by running `git rev-parse --git-dir`
   - If not in a git repo, inform the user and stop
   - List all existing worktrees using `git worktree list`
   - Verify that the specified worktree exists in the list

3. **Verify the worktree exists**:
   - Check if `../worktrees/$ARGUMENTS` exists
   - If the worktree doesn't exist, inform the user and show the list of available worktrees
   - If it exists but is not in git's worktree list, this is an inconsistent state - warn the user

4. **Check for uncommitted changes**:
   - Before removing, check if the worktree has uncommitted changes
   - If it does, warn the user and ask for confirmation
   - Offer the option to force remove with `--force` flag

5. **Remove the worktree**:
   - Execute: `git worktree remove ../worktrees/$ARGUMENTS`
   - If there are uncommitted changes and the user confirmed, use: `git worktree remove --force ../worktrees/$ARGUMENTS`
   - Note: `git worktree remove` automatically cleans up the directory

6. **Provide feedback**:
   - Inform the user that the worktree was successfully removed
   - Display the name of the removed worktree
   - Optionally show the remaining worktrees

## Error Handling

- **No worktree name provided**: List existing worktrees and ask which one to remove
- **Not in a git repository**: Inform the user they need to be in a git repository
- **Worktree doesn't exist**: Show list of available worktrees
- **Uncommitted changes**: Warn the user and ask for confirmation before force removing
- **Currently in the worktree to be removed**: Inform the user they need to navigate to a different directory first
- **Permission errors**: Inform the user about permission issues with removing directories

## Example Usage

User: `/git-worktree:remove-worktree feature-xyz`

Expected behavior:
1. Verify `../worktrees/feature-xyz` exists in git worktree list
2. Check for uncommitted changes
3. Remove the worktree using `git worktree remove`
4. Confirm successful removal

## Safety Features

- Always check for uncommitted changes before removing
- Require user confirmation if there are uncommitted changes
- Prevent removal if the user is currently in the worktree being removed
- Provide clear feedback about what was removed
