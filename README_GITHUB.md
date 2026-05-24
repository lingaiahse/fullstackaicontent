# GitHub and GitHub Actions Overview for Development

## Who this guide is for
If you are new to GitHub, version control, and CI/CD, this guide is written for you.
It explains the basics of repositories, branches, pull requests, and automation.

## What you'll learn
- How GitHub works and why it is useful
- How to create and manage repositories
- Collaboration workflows like pull requests and issues
- Intro to GitHub Actions for continuous integration


# 1. GitHub Overview

## What is GitHub?
Definition: GitHub is a web-based platform for hosting Git repositories, collaborating on code, and managing software development workflows.

Key Features:
- Source code hosting
- Branch and merge workflows
- Pull requests and code reviews
- Issues and project boards
- Actions for automation

Real-world Use Case: Teams use GitHub to manage code, track bugs, and deploy applications with CI/CD pipelines.

## Why Use GitHub?
Definition: GitHub centralizes version control and developer collaboration with a powerful interface and integrations.

Advantages:
- Distributed version control with Git
- Built-in collaboration tools
- Strong ecosystem and marketplace

Disadvantages:
- Public repositories are visible unless paid private repos are used (now mostly free with limits)
- Learning Git commands can be a barrier for beginners

Use Case: Open source projects, enterprise development, and distributed teams.

## Git vs GitHub
Definition: Git is a version control system; GitHub is a hosting service built around Git.

Example:
- Git manages commits, branches, and diffs locally
- GitHub stores repositories remotely and adds collaboration features

## GitHub Portal Overview
Definition: The GitHub web portal provides dashboards for repositories, organizations, and developer activities.

Portal Sections:
- Repositories: Code, branches, commits, and PRs
- Issues: Bugs, tasks, and feature requests
- Pull requests: Review, discuss, and merge changes
- Actions: Automated workflows and CI/CD
- Projects: Kanban boards and planning
- Security: Dependabot, vulnerability alerts, code scanning
- Insights: Contributor activity and repo metrics

Use Case: Developers use the portal daily to review pull requests, merge code, and monitor builds.

---

# 2. GitHub Repository Basics

## Repository Structure
Definition: A repository contains code, history, branches, and configuration files.

Common Files:
- `README.md`: Project overview and instructions
- `.gitignore`: Files excluded from version control
- `LICENSE`: Open source license file
- `package.json` / `requirements.txt`: Dependency and project metadata

## Creating a Repository
Definition: Start a new GitHub repo through the portal or CLI.

Example CLI:
```bash
git init
git remote add origin https://github.com/user/repo.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

## Cloning a Repository
Definition: Copy a remote repository locally.

Example:
```bash
git clone https://github.com/user/repo.git
```

## Branching
Definition: Create independent lines of development.

Example:
```bash
git checkout -b feature/login
```

## Commits
Definition: Save snapshots of your changes.

Example:
```bash
git add .
git commit -m "Add login page"
```

## Pushing and Pulling
Definition: Send and fetch updates from the remote repo.

Example:
```bash
git push origin feature/login
git pull origin main
```

## Forking
Definition: Create your own copy of a repository to contribute back.

Use Case: Open source contributions and experimentation.

---

# 3. Collaboration Workflows

## Pull Requests
Definition: Proposed code changes submitted for review before merging.

Key Steps:
1. Create a branch
2. Push changes
3. Open a pull request (PR)
4. Review and discuss
5. Merge to main or target branch

## Code Review
Definition: Reviewers inspect diffs, leave comments, and approve or request changes.

Best Practices:
- Review small, focused PRs
- Use line comments and suggestions
- Ensure tests pass before merging

## Issues
Definition: Track bugs, features, and tasks with GitHub Issues.

Use Case: Create issue templates for bug reports and feature requests.

## Labels
Definition: Categorize issues and PRs with labels.

Use Case: `bug`, `enhancement`, `help wanted`, `documentation`.

## Milestones
Definition: Group issues and PRs by release or sprint.

Use Case: Track progress toward version releases.

## Project Boards
Definition: Kanban-style boards for workflow planning.

Use Case: Organize tasks, in progress work, and review status.

---

# 4. GitHub Portal for Development

## Repository Page
Definition: Main view with code, branches, pull requests, and releases.

Important Tabs:
- Code: Browse files and view README
- Issues: Open and manage issues
- Pull requests: Review pending changes
- Actions: View workflow runs
- Projects: Track development workflows
- Security: Review vulnerabilities and alerts
- Insights: Visualize contributions and activity

## Notifications
Definition: Alerts for mentions, PR updates, and issue activity.

Use Case: Stay updated on team changes and reviews.

## Security & Code Scanning
Definition: Built-in tools for dependency scanning and static analysis.

Use Case: Detect vulnerabilities with Dependabot and GitHub Advanced Security.

## Wiki
Definition: Publish documentation and guides inside the repository.

Use Case: Maintain onboarding docs, architecture notes, and API references.

## Releases
Definition: Package stable versions with release notes.

Use Case: Distribute software versions and changelogs.

## GitHub Pages
Definition: Host static websites directly from a repository.

Use Case: Project docs, portfolios, and landing pages.

---

# 5. GitHub Actions Overview

## What is GitHub Actions?
Definition: GitHub Actions is an automation platform that runs workflows in response to events like pushes, PRs, or scheduled tasks.

Advantages:
- Integrated with GitHub repositories
- Supports CI/CD, testing, deployment, and automation
- Large marketplace of reusable actions

Disadvantages:
- Workflow complexity can grow quickly
- Usage quotas may apply for large projects

Use Case: Automatically build and deploy apps when code is merged.

## Workflow Basics
Definition: A workflow is a YAML file that defines jobs and steps executed on GitHub-hosted runners.

Structure:
- `on`: Event triggers
- `jobs`: Independent job definitions
- `steps`: Actions and commands

Example:
```yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
```

## Workflow Triggers
Definition: Events that start workflows.

Common Triggers:
- `push`
- `pull_request`
- `workflow_dispatch`
- `schedule`
- `release`

Use Case: Run CI on PRs and nightly tests on schedule.

## Jobs and Steps
Definition: Jobs run on separate runners and contain ordered steps.

Example:
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm test
```

## Runners
Definition: Virtual machines or containers that execute workflow jobs.

Types:
- GitHub-hosted runners
- Self-hosted runners

Use Case: Use self-hosted runners for custom hardware or private networks.

---

# 6. GitHub Actions Concepts

## Actions
Definition: Reusable units that perform tasks in workflows.

Example:
- `actions/checkout@v4`
- `actions/setup-node@v5`

## Secrets
Definition: Secure environment variables stored in the repo or org.

Use Case: Store API keys and credentials for deployment.

## Matrices
Definition: Run jobs across multiple environments.

Example:
```yaml
strategy:
  matrix:
    node-version: [16, 18]
```

## Caching
Definition: Cache dependencies to speed up builds.

Example:
```yaml
- name: Cache node modules
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

## Artifacts
Definition: Store build outputs or test reports.

Use Case: Download logs or packaged apps from workflow runs.

## Environments
Definition: Define deployment targets like staging and production.

Use Case: Protect production with required reviews and secrets.

---

# 7. Example GitHub Actions Workflows

## Node.js CI Workflow
Example:
```yaml
name: Node CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18.x, 20.x]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v5
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm test
```

## Build and Deploy Workflow
Example:
```yaml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

## Scheduled Workflow
Example:
```yaml
name: Nightly Tests
on:
  schedule:
    - cron: '0 2 * * *'
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm test
```

---

# 8. GitHub Portal Features for Development

## Code Review Interface
Definition: Review code changes with inline comments, suggestions, and approval requirements.

Use Case: Quality control and knowledge sharing.

## Pull Request Templates
Definition: Predefined PR descriptions to standardize submissions.

Use Case: Ensure reviewers get context and test instructions.

## Issue Templates
Definition: Structured issue forms for bugs, features, and tasks.

Use Case: Improve issue quality and triage.

## Projects and Roadmaps
Definition: Track work using project boards, issues, and automation.

Use Case: Manage sprints, releases, and team priorities.

## Dependabot
Definition: Automated dependency updates and security alerts.

Use Case: Keep projects up to date and secure.

## Code Owners
Definition: Assign responsible reviewers based on file paths.

Use Case: Enforce review rules for critical code areas.

## Discussions
Definition: Forum-style collaboration for ideas, questions, and announcements.

Use Case: Community feedback and design discussions.

## GitHub CLI
Definition: Command line tool for interacting with GitHub from terminal.

Example:
```bash
gh repo clone user/repo
gh pr create --fill
gh issue create --title "Bug" --body "Details"
```

---

# 9. Best Practices for GitHub Development

## Branch Strategy
Definition: Use branching models like GitHub Flow, Git Flow, or Trunk Based Development.

Use Case: Keep main branch stable while enabling feature work.

## Commit Messages
Definition: Write clear, descriptive commit messages.

Example:
```
feat(auth): add login endpoint
fix(ui): correct button spacing
```

## Pull Request Size
Definition: Keep PRs small and focused for easier review.

## Continuous Integration
Definition: Run tests and linting on every push and PR.

## Protected Branches
Definition: Require reviews, passing checks, and status checks before merges.

Use Case: Maintain code quality and reduce regressions.

---

# 10. GitHub Actions Best Practices

## Secure Secrets
Definition: Store tokens and credentials in GitHub Secrets.

## Fail Fast
Definition: Stop workflows early when tests fail.

## Reuse Workflows
Definition: Create reusable workflow files for common patterns.

## Monitor Workflow Runs
Definition: Use the Actions tab to inspect logs, failures, and timing.

## Use Marketplace Actions Carefully
Definition: Prefer trusted actions with good maintenance and community usage.

---

# 11. GitHub Portal for Teams

## Organizations
Definition: Group repositories, teams, and permissions under one account.

## Teams
Definition: Manage access and review assignments by team.

## Permissions
Definition: Control read, triage, write, maintain, and admin access.

## Audit Logs
Definition: Track organization activity and security events.

Use Case: Enterprise-scale collaboration and access control.

---

# 12. GitHub Integration and Ecosystem

## GitHub Marketplace
Definition: Apps and actions to extend GitHub workflows.

## Webhooks
Definition: Trigger external services on GitHub events.

## Third-Party Integrations
Examples:
- Slack
- Jira
- Sentry
- Azure DevOps

## GitHub API
Definition: Automate repository management and query GitHub data.

Example:
```bash
curl -H "Authorization: token $TOKEN" https://api.github.com/repos/user/repo
```

---

# 13. GitHub for Development Need

## Developer Onboarding
Definition: Use README, CONTRIBUTING, and docs to help new contributors.

Use Case: Onboard team members quickly with clear guidelines.

## Code Quality
Definition: Enforce linting, formatting, and review rules.

## Deployment Pipelines
Definition: Automate builds, tests, and deployments with Actions.

## Monitoring and Alerts
Definition: Use status checks, issue tracking, and security alerts.

## Collaboration
Definition: Use issues, PRs, projects, and discussions to coordinate work.

---

# 14. GitHub Interview and Learning Topics

## Common Topics
- Git vs GitHub
- Branching and merging
- Pull request workflows
- GitHub Actions basics
- Protecting branches
- Issue and project management
- Security and code scanning

## Practical Skills
- Create PRs and review code
- Write GitHub Actions workflows
- Configure environments and secrets
- Use GitHub CLI effectively

---

# Recommended Learning Order

1. Git fundamentals
2. GitHub repository basics
3. Branching and PR workflows
4. GitHub portal features
5. Issues and projects
6. GitHub Actions CI/CD
7. Security and protected branches
8. Team collaboration with organizations and teams

---

# Practice Projects

- Open source contribution workflow
- CI/CD pipeline for a web app
- Issue board for sprint planning
- Automated release workflow with Actions
- GitHub Pages documentation site

---

# Official Resources

- [GitHub Docs](https://docs.github.com/)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [GitHub Learning Lab](https://lab.github.com/)

---

# 15. Git and GitHub Commands

## Git Basics
Definition: Core Git commands manage repository history, branches, commits, and collaboration.

### Initialize repository
```bash
git init
```

### Clone repository
```bash
git clone https://github.com/user/repo.git
```

### Check status
```bash
git status
```

### Add changes
```bash
git add .
```

### Commit changes
```bash
git commit -m "Add feature"
```

### View history
```bash
git log --oneline --graph --decorate
```

### Show differences
```bash
git diff
```


## Branching Commands
Definition: Branch commands create and switch contexts for features and fixes.

### Create branch
```bash
git branch feature/login
```

### Switch branch
```bash
git checkout feature/login
```

### Create and switch
```bash
git checkout -b feature/login
```

### List branches
```bash
git branch
```

### Delete branch
```bash
git branch -d feature/login
```

### Rename branch
```bash
git branch -m old-name new-name
```


## Remote Commands
Definition: Remote commands synchronize local changes with GitHub and other remotes.

### Add remote
```bash
git remote add origin https://github.com/user/repo.git
```

### Show remotes
```bash
git remote -v
```

### Push branch
```bash
git push origin feature/login
```

### Set upstream on push
```bash
git push -u origin feature/login
```

### Fetch remote
```bash
git fetch origin
```

### Pull remote changes
```bash
git pull origin main
```

### Push tags
```bash
git tag v1.0
git push origin v1.0
```


## Merge and Rebase
Definition: Integrate changes from one branch into another safely.

### Merge branch
```bash
git checkout main
git merge feature/login
```

### Rebase branch
```bash
git checkout feature/login
git rebase main
```

### Abort rebase
```bash
git rebase --abort
```

### Resolve conflicts
```bash
# edit conflicted files
git add <file>
git rebase --continue
```


## Stashing and Resetting
Definition: Temporarily save changes or reset history.

### Stash changes
```bash
git stash
```

### Apply stash
```bash
git stash pop
```

### Discard local changes
```bash
git checkout -- <file>
```

### Reset staged files
```bash
git reset HEAD <file>
```

### Hard reset branch
```bash
git reset --hard HEAD~1
```


## Tagging and Releases
Definition: Mark versions and create release points.

### Create lightweight tag
```bash
git tag v1.0
```

### Create annotated tag
```bash
git tag -a v1.0 -m "Release 1.0"
```

### Push tags
```bash
git push origin --tags
```


## GitHub CLI Commands
Definition: Use `gh` to interact with GitHub from the terminal for PRs, issues, workflows, and repo management.

### Authenticate
```bash
gh auth login
```

### Clone repository
```bash
gh repo clone owner/repo
```

### Create pull request
```bash
gh pr create --base main --head feature/login --title "Add login" --body "Implements login flow"
```

### View open PRs
```bash
gh pr list
```

### Check out PR locally
```bash
gh pr checkout 42
```

### Merge PR
```bash
gh pr merge 42 --merge
```

### Create issue
```bash
gh issue create --title "Bug" --body "Describe the bug"
```

### List issues
```bash
gh issue list
```

### View workflow runs
```bash
gh run list
```

### Trigger workflow dispatch
```bash
gh workflow run ci.yml --ref main
```

### Create repo
```bash
gh repo create my-repo --public
```

### Fork repo
```bash
gh repo fork owner/repo
```

### Manage releases
```bash
gh release create v1.0 --notes "Initial release"
```


## Useful Git Aliases
Definition: Shortcuts to speed up frequent Git commands.

Example:
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm "commit -m"
```


## Command Best Practices
- Commit early and often with meaningful messages.
- Use feature branches for isolated work.
- Rebase locally before merging to keep history clean.
- Use `git status` and `git diff` to verify changes before push.
- Keep remote branches in sync with `git fetch` and `git pull`.
- Protect main branches with reviews and CI checks.
