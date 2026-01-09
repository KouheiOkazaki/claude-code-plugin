---
name: add-git-worktree
description: Creates a new git worktree with a specified name in ../worktrees/<name> directory and automatically navigates to it. Use when the user wants to create a new worktree for parallel development.
---

# Add Git Worktree Skill

This skill creates a new git worktree with the specified name and automatically navigates to it.

## Workflow

When the user invokes this skill with a worktree name (e.g., `/claude-code-plugin:add-git-worktree feature-branch`):

1. **Validate the input**:
   - Check that the user provided a worktree name in `$ARGUMENTS`
   - If no name is provided, ask the user to provide a worktree name
   - The worktree name will also be used as the branch name

2. **Check prerequisites**:
   - Verify we're in a git repository by running `git rev-parse --git-dir`
   - If not in a git repo, inform the user and stop
   - Check if a worktree with the same name already exists by running `git worktree list`
   - If it exists, inform the user and suggest using a different name

3. **Create the worktrees parent directory if needed**:
   - Check if `../worktrees/` directory exists
   - If not, create it using `mkdir -p ../worktrees`

4. **Create the worktree**:
   - Execute: `git worktree add ../worktrees/$ARGUMENTS -b $ARGUMENTS`
   - This creates a new worktree at `../worktrees/<name>` with a new branch `<name>`
   - If the branch name already exists, inform the user and ask if they want to use the existing branch or choose a different name

5. **Navigate to the worktree**:
   - Change the working directory to the newly created worktree: `cd ../worktrees/$ARGUMENTS`
   - Confirm the current location by running `pwd`

6. **Provide feedback**:
   - Inform the user that the worktree was successfully created
   - Display the full path to the new worktree
   - Display the branch name
   - Inform the user that you've navigated to the new worktree and they can now start working

## Error Handling

- **No worktree name provided**: Ask the user to provide a name
- **Not in a git repository**: Inform the user they need to be in a git repository
- **Worktree already exists**: Suggest using a different name or removing the existing worktree first
- **Branch name conflict**: Ask if they want to checkout the existing branch in a new worktree location
- **Permission errors**: Inform the user about permission issues with creating directories

## Example Usage

User: `/claude-code-plugin:add-git-worktree feature-xyz`

Expected behavior:
1. Create `../worktrees/feature-xyz` directory
2. Create new branch `feature-xyz`
3. Navigate to `../worktrees/feature-xyz`
4. Confirm success with the path and branch information
