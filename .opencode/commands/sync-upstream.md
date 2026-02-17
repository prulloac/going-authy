---
description: Sync with upstream, rebase, commit changes, and push
agent: general
---

# Sync with Upstream Repository

You are tasked with synchronizing the local repository with the upstream remote repository. Follow these steps in order:

## Step 1: Check for Missing Commits from Upstream
Run the following command to check if there are missing commits from the upstream remote:

!`git fetch upstream && git log --oneline HEAD..upstream/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null || git log --oneline HEAD..origin/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null || echo "No upstream tracking branch found"`

If there are commits listed above, proceed to Step 2. If there are no missing commits, inform the user that the local repository is already up to date with the upstream.

## Step 2: Rebase onto Upstream (if needed)
If missing commits were found, rebase the local repository onto the upstream branch:

!`git rebase upstream/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null || git rebase origin/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null || echo "Could not determine upstream branch"`

If the rebase fails due to conflicts, ask the user to resolve the conflicts manually and then re-run this command.

## Step 3: Check for Local Changes (Always Perform)
Regardless of whether a rebase was performed, always check for new local changes to be committed:

!`git status`

If there are staged or unstaged changes, proceed to Step 4. Otherwise, continue to Step 5.

## Step 4: Create Commits Using git-commit-workflow (if changes found)
Use the git-commit-workflow to analyze all changes and create well-structured commits:
- Review all staged and unstaged changes
- Group related changes logically
- Follow repository-specific commit message guidelines (or use conventional commits as fallback)
- Create commits that are logical and well-organized
- Ensure the local repository is now ahead of the remote repository with proper commits

## Step 5: Push to Upstream
After commits are created, push the changes to the upstream branch:

!`git push --set-upstream upstream $(git rev-parse --abbrev-ref HEAD) 2>/dev/null || git push --set-upstream origin $(git rev-parse --abbrev-ref HEAD)`

Verify that the push was successful and inform the user of the final status.

## Summary
After completion, provide the user with:
1. Number of commits rebased from upstream (if any)
2. Number of new commits created locally (if any)
3. Confirmation that changes have been pushed to the upstream branch
4. Any issues or warnings encountered during the process
