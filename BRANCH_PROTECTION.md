# Branch Protection Rules Documentation

## Overview

This document outlines the branch protection rules implemented for the Weather Dashboard DevOps project. These rules ensure code quality, prevent accidental changes, and enforce proper collaboration workflows.

---

## Protected Branches

### 1. `main` Branch (Production)

**Protection Rules:**
- ✅ **Require pull request before merging**
  - At least 1 approval required
  - Dismiss stale reviews when new commits are pushed
  
- ✅ **Require status checks to pass**
  - CI Pipeline must pass
  - All tests must succeed
  
- ✅ **Require branches to be up to date**
  - Must be current with main before merging
  
- ✅ **Restrict direct pushes**
  - No one can push directly to main
  - All changes must come through Pull Requests
  
- ✅ **Require signed commits** (Optional)
  - For enhanced security

- ✅ **Include administrators**
  - Rules apply to everyone

---

## How to Configure Branch Protection on GitHub

### Step-by-Step Instructions:

1. Go to your GitHub repository
2. Click **Settings** tab
3. Navigate to **Branches** under "Code and automation"
4. Click **Add rule** next to "Branch protection rules"
5. Enter `main` in "Branch name pattern"
6. Enable the following options:

```
☑️ Require a pull request before merging
   ☑️ Require approvals (1)
   ☑️ Dismiss stale pull request approvals when new commits are pushed
   
☑️ Require status checks to pass before merging
   ☑️ Require branches to be up to date before merging
   - Add required status checks: "build"
   
☑️ Do not allow bypassing the above settings

☑️ Restrict pushes that create matching branches
```

7. Click **Create** to save the rule

---

## Branch Workflow Diagram

```
feature/xxx  ──────────────────────────────────────────┐
                                                       │
                                                       ▼
feature/yyy  ─────────────────────┐               [Pull Request]
                                  │                    │
                                  ▼                    │
feature/zzz  ─────────┐      [Pull Request]           │
                      │           │                    │
                      ▼           ▼                    ▼
                  [Pull Request] ─── develop ─── [Pull Request] ─── main ─── 🚀 Deploy
                                         ▲                             │
                                         │                             │
                                   Integration                    Production
                                    Branch                         Branch
```

---

## Branch Naming Conventions

| Branch Type | Naming Pattern | Example |
|-------------|---------------|---------|
| Production | `main` | `main` |
| Development | `develop` | `develop` |
| Feature | `feature/description` | `feature/weather-api` |
| Bug Fix | `fix/description` | `fix/search-error` |
| Hotfix | `hotfix/description` | `hotfix/api-timeout` |
| Documentation | `docs/description` | `docs/readme-update` |

---

## Merge Conflict Resolution

### When Conflicts Occur:

1. **Pull latest changes:**
   ```bash
   git checkout develop
   git pull origin develop
   git checkout your-feature-branch
   git merge develop
   ```

2. **Resolve conflicts in editor:**
   - Look for conflict markers: `<<<<<<<`, `=======`, `>>>>>>>`
   - Keep the code you want
   - Remove conflict markers

3. **Commit resolution:**
   ```bash
   git add .
   git commit -m "fix: resolve merge conflicts with develop"
   git push origin your-feature-branch
   ```

4. **Document the conflict:**
   - Add a comment in your Pull Request describing what was conflicting
   - Explain your resolution approach

---

## Enforcement Summary

| Rule | main | develop | feature/* |
|------|------|---------|-----------|
| Direct push allowed | ❌ | ⚠️ Limited | ✅ |
| Requires PR | ✅ | ✅ | ❌ |
| Requires review | ✅ | ⚠️ Optional | ❌ |
| CI must pass | ✅ | ✅ | ✅ |
| Auto-deploy | ✅ | ❌ | ❌ |

---

## CI/CD Integration with Branch Protection

When a PR is created:

1. **GitHub Actions triggers automatically:**
   - Runs `npm install`
   - Runs `npm run lint`
   - Runs `npm run build`
   - Runs `npm test`

2. **Status check appears on PR:**
   - ✅ Green check = All tests passed
   - ❌ Red X = Something failed

3. **Merge is blocked if:**
   - CI checks fail
   - No approvals
   - Branch is behind target

4. **On merge to main:**
   - Deployment workflow triggers
   - App deploys to Netlify automatically

---

## Quick Reference Commands

```bash
# Create feature branch
git checkout develop
git pull origin develop
git checkout -b feature/new-feature

# Push feature branch
git push -u origin feature/new-feature

# After PR approval, merge to develop
git checkout develop
git merge feature/new-feature --no-ff
git push origin develop

# Deploy to production (merge develop to main)
git checkout main
git merge develop
git push origin main
# Auto-deployment triggers!
```

---

## Security Considerations

1. **Never commit API keys** to the repository
2. **Use GitHub Secrets** for sensitive data
3. **Review dependencies** before installing
4. **Keep branches up to date** to avoid conflicts

---

*Last Updated: January 2025*
*Document Version: 1.0*
