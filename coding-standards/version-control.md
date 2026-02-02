# Version Control Guidelines

## Overview

We use Git for version control across all projects. These guidelines ensure consistent and effective use of version control.

## Git Workflow

### Branch Strategy

We follow a **Git Flow** branching model:

```
main (production)
  ↑
develop (integration)
  ↑
feature/* (new features)
bugfix/* (bug fixes)
hotfix/* (urgent production fixes)
release/* (release preparation)
```

### Branch Naming

**Feature Branches:**
```
feature/user-authentication
feature/payment-integration
feature/PROJ-123-add-dashboard
```

**Bug Fix Branches:**
```
bugfix/login-error
bugfix/PROJ-456-fix-memory-leak
```

**Hotfix Branches:**
```
hotfix/security-patch
hotfix/critical-api-error
```

**Release Branches:**
```
release/v1.2.0
release/2024-02-sprint-1
```

### Branch Lifecycle

1. **Create Branch**: Branch from `develop` (or `main` for hotfixes)
2. **Develop**: Make commits following commit message guidelines
3. **Push**: Push to remote regularly
4. **Pull Request**: Create PR when ready for review
5. **Review**: Address review comments
6. **Merge**: Merge after approval
7. **Delete**: Delete branch after merge

## Commit Messages

### Format

Follow the **Conventional Commits** specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, missing semicolons, etc.)
- **refactor**: Code refactoring
- **perf**: Performance improvements
- **test**: Adding or updating tests
- **build**: Build system changes
- **ci**: CI/CD configuration changes
- **chore**: Other changes (dependencies, etc.)

### Examples

**Simple commit:**
```
feat(auth): add JWT authentication

Implemented JWT-based authentication for API endpoints.
Includes token generation and validation middleware.
```

**With issue reference:**
```
fix(api): resolve memory leak in user service

Fixed memory leak caused by unclosed database connections.
Added proper connection cleanup in finally block.

Fixes #234
```

**Breaking change:**
```
feat(api): redesign user API endpoints

BREAKING CHANGE: User API endpoints have been redesigned.
- Changed /users/:id to /api/v2/users/:id
- Response format updated to include metadata

Migration guide available in docs/migration.md

Closes #567
```

### Best Practices

**DO:**
- Write in imperative mood ("add feature" not "added feature")
- Keep subject line under 50 characters
- Capitalize subject line
- Don't end subject line with a period
- Separate subject from body with a blank line
- Wrap body at 72 characters
- Explain what and why, not how

**DON'T:**
- Make commits too large (prefer smaller, atomic commits)
- Mix unrelated changes in one commit
- Commit broken or untested code
- Use vague messages like "fix bug" or "update code"

## Pull Requests

### Creating PRs

**Title Format:**
```
[TYPE] Brief description
```

Examples:
```
[FEAT] Add user authentication
[FIX] Resolve login timeout issue
[DOCS] Update API documentation
```

### PR Description Template

```markdown
## Description
[Brief description of changes]

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Fixes #[issue number]
Related to #[issue number]

## Changes Made
- [List of changes]
- [Another change]

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] No new warnings
- [ ] Tests added/updated
```

### PR Best Practices

- Keep PRs focused and small (< 400 lines)
- Provide clear description and context
- Reference related issues
- Include screenshots for UI changes
- Request specific reviewers
- Respond to review comments promptly
- Keep PR up-to-date with base branch

## Git Best Practices

### Daily Workflow

1. **Start of Day:**
```bash
git checkout develop
git pull origin develop
git checkout -b feature/my-feature
```

2. **During Development:**
```bash
# Make changes
git add .
git commit -m "feat(module): add new feature"

# Push regularly
git push origin feature/my-feature
```

3. **Keep Updated:**
```bash
git checkout develop
git pull origin develop
git checkout feature/my-feature
git merge develop
# or
git rebase develop
```

4. **End of Day:**
```bash
git push origin feature/my-feature
```

### Useful Commands

**Check Status:**
```bash
git status
git log --oneline --graph --all
```

**Undo Changes:**
```bash
# Undo unstaged changes
git checkout -- file.txt

# Unstage files
git reset HEAD file.txt

# Amend last commit
git commit --amend

# Reset to previous commit (keep changes)
git reset --soft HEAD~1

# Reset to previous commit (discard changes)
git reset --hard HEAD~1
```

**Stash Changes:**
```bash
# Save work in progress
git stash save "WIP: feature description"

# List stashes
git stash list

# Apply stash
git stash apply stash@{0}

# Apply and remove stash
git stash pop
```

**Clean Up:**
```bash
# Delete local branch
git branch -d feature/my-feature

# Delete remote branch
git push origin --delete feature/my-feature

# Prune deleted remote branches
git fetch --prune
```

## Merge Strategies

### When to Use Each

**Merge Commit:**
- Preserves complete history
- Good for feature branches
```bash
git merge --no-ff feature/my-feature
```

**Squash and Merge:**
- Cleaner history
- Good for small features with many commits
```bash
git merge --squash feature/my-feature
git commit -m "feat: add new feature"
```

**Rebase:**
- Linear history
- Good for keeping feature branches updated
```bash
git rebase develop
```

### Our Strategy

- **Feature → Develop**: Squash and merge
- **Develop → Main**: Merge commit
- **Hotfix → Main**: Merge commit

## Protecting Branches

### Main/Master Branch

Protected with these rules:
- Require pull request reviews (min 1)
- Require status checks to pass
- Require branches to be up to date
- No force push
- No deletions

### Develop Branch

Protected with these rules:
- Require pull request reviews
- Require status checks to pass
- No force push

## Tags and Releases

### Semantic Versioning

Follow [SemVer](https://semver.org/): `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

### Creating Tags

```bash
# Annotated tag
git tag -a v1.2.0 -m "Release version 1.2.0"

# Push tag
git push origin v1.2.0

# Push all tags
git push --tags
```

### Release Process

1. Create release branch from develop
2. Update version numbers
3. Update CHANGELOG
4. Test thoroughly
5. Merge to main and develop
6. Tag the release
7. Deploy to production
8. Create GitHub release with notes

## .gitignore

### Essential Items

```gitignore
# Dependencies
node_modules/
vendor/
*.egg-info/

# Environment
.env
.env.local
*.key
*.pem

# IDE
.vscode/
.idea/
*.swp

# Build output
dist/
build/
*.pyc
*.class

# OS
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Test coverage
coverage/
.coverage
```

## Git Configuration

### User Setup

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@solution8.com"
```

### Useful Aliases

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --oneline --graph --all'
```

### Line Endings

```bash
# Windows
git config --global core.autocrlf true

# Mac/Linux
git config --global core.autocrlf input
```

## Security

### Never Commit:

- Passwords or API keys
- Private keys or certificates
- Database connection strings
- Personal access tokens
- Customer data

### If You Accidentally Commit Secrets:

1. **Immediately** rotate the exposed credentials
2. Remove from Git history:
```bash
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/file" \
  --prune-empty --tag-name-filter cat -- --all
```
3. Force push (if allowed)
4. Notify security team

### Use Git-secrets

Install and configure git-secrets to prevent commits with secrets:

```bash
git secrets --install
git secrets --register-aws
```

## Troubleshooting

### Merge Conflicts

```bash
# See conflicted files
git status

# Open file and resolve conflicts between <<<< and >>>>
# After resolving:
git add resolved-file.txt
git commit
```

### Accidentally Committed to Wrong Branch

```bash
# Don't push yet!
git reset HEAD~1
git stash
git checkout correct-branch
git stash pop
git add .
git commit
```

### Recover Deleted Commits

```bash
# Find lost commit
git reflog

# Recover it
git cherry-pick <commit-hash>
```

## Resources

- [Git Documentation](https://git-scm.com/doc)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
- [Semantic Versioning](https://semver.org/)
