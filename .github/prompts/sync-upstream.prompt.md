---
description: Sync with upstream, rebase, commit changes, and push
agent: general
tools: []
---

# Sync with Upstream Repository

You are tasked with synchronizing the local repository with the upstream remote repository. Follow these steps systematically:

## Step 1: Check for Missing Commits from Upstream

First, fetch from upstream and check if there are missing commits that need to be rebased:

```bash
git fetch upstream
git log --oneline HEAD..upstream/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null || git log --oneline HEAD..origin/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null
```

**Action:**
- If there are commits listed above, proceed to Step 2
- If there are no missing commits, inform the user that the local repository is already up to date
- If you cannot determine the upstream branch, ask the user to clarify the upstream remote name

## Step 2: Rebase onto Upstream (if needed)

If missing commits were found, rebase the local commits onto the upstream branch:

```bash
git rebase upstream/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null || git rebase origin/$(git rev-parse --abbrev-ref HEAD) 2>/dev/null
```

**Action:**
- If the rebase succeeds, proceed to Step 3
- If the rebase fails due to conflicts, ask the user to resolve the conflicts manually and then re-run this command

## Step 3: Check for Local Changes (Always Perform)

Regardless of rebasing, always check for any local changes that need to be committed:

```bash
git status
git diff --cached
git diff
```

**Action:**
- If there are staged or unstaged changes, proceed to Step 4
- Otherwise, continue to Step 5

## Step 4: Create Commits Using git-commit-workflow (if changes found)

Analyze all local changes (whether or not a rebase was required) and create well-structured commits following the git-commit-workflow:

1. **Read Repository Guidelines** - Look for CONTRIBUTING.md or similar files
2. **Assess Current Changes** - Review both staged and unstaged changes
3. **Analyze Changes in Detail** - Use `git diff` to understand specifics
4. **Group Related Changes** - Logically group files by related functionality
5. **Perform Commits** - Create commits following guidelines or conventional commits

Ensure:
- Commits follow repository-specific guidelines
- Each commit has a clear, descriptive message
- Related changes are grouped together logically
- The local repository is now ahead of the remote with proper commits

## Step 5: Push to Upstream

Push the changes to the upstream branch:

```bash
git push --set-upstream upstream $(git rev-parse --abbrev-ref HEAD) 2>/dev/null || git push --set-upstream origin $(git rev-parse --abbrev-ref HEAD)
```

**Action:**
- Verify the push was successful
- Inform the user of the final status

## Final Summary

After completion, provide the user with a summary including:
- Number of commits rebased from upstream (if any)
- Number of new commits created locally (if any)
- Confirmation that all changes have been pushed to the upstream branch
- Any warnings or issues encountered during the process
