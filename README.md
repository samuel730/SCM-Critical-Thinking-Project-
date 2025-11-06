# Source-Code-Management-Project-3

Task 1 – Evaluate SCM Tools
Deliverable: Report skeleton (markdown)

1.1 Centralized vs. Distributed
| Feature                | Centralized (SVN)                     | Distributed (Git)                     |
|------------------------|----------------------------------------|----------------------------------------|
| Repository model   | Single server, clients pull/checkout   | Every clone is a full repo              |
| Network dependency| Must talk to server for most actions   | Offline work; only push/pull needs net   |
| Branching cost     | Heavy, creates server-side folders      | Light, instant branch/merge             |
| Merge strategy     | File-level lock or manual merge         | Automatic 3-way merge; conflict-aware   |
| Speed              | Slower for large histories             | Fast (local history, cheap diffs)       |
| Access control     | Central ACLs, easier enforcement       | Distributed; can enforce via hooks/PRs   |

1.2 Why Git for Distributed Teams
- Local dev & offline work – devs can commit, branch, experiment anywhere.
- Fast collaboration – Push/pull to remote, no single point of failure.
- Rich ecosystem – GitHub, GitLab, Bitbucket, CI/CD integrations.
- Built-in code review – Pull/Merge Requests + status checks.

1.3 Challenges & Mitigation
- Learning curve – Provide training, internal cheat sheets.
- Repo size – Use shallow clones, LFS for binaries.
- Security – Enforce SSH keys, GPG signing, branch protection.

Task 2 – Git Workflow Guide
2.1 Branching Model (Git Flow + PR)

main → production
develop → integration
feature/* → new features
hotfix/* → urgent fixes
release/* → pre-prod staging


2.2 Typical Flow
1. Create feature

bash
git checkout develop
git pull
git checkout -b feature/login-api

2. Work, commit, push

bash
git add .
git commit -m "feat: add login endpoint"
git push -u origin feature/login-api

3. Open PR → Assign reviewer, add PR template.
4. CI checks (see Task 3) → Must pass lint, test, build.
5. Merge → Squash & merge into develop, delete branch.

2.3 Best Practices
- Small, focused PRs (<200 LOC).
- Rebase before PR to keep history clean.
- Delete merged branches to avoid clutter.

Task 3 – CI/CD Pipeline (GitHub Actions)

name: CI/CD Pipeline
on:
  push:
    branches: [ feature/*, hotfix/* ]
  pull_request:
    branches: [ develop, main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install deps
        run: npm ci
      - name: Lint
        run: npm run lint
      - name: Test
        run: npm test -- --coverage
      - name: Build
        run: npm run build
      - name: Deploy to Staging
        if: github.ref == 'refs/heads/develop'
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist


How it works:
- Trigger on every push to feature branches & PRs to develop/main.
- Lint → Test → Build → Deploy to staging if on develop.
- Add status badge to README.md for visibility.

 Task 4 – Security Best Practices
4.1 Enable Branch Protection
- Require PR before merging to main/develop.
- Require status checks Settings (CI must pass).
- Sign commits with GPG.

4.2 SSH & MFA
- Rotate SSH keys every 90 days.
- Enforce MFA for all devs.

4.3 Audit
- Enable GitHub Audit Log → export to SIEM.

4.4 Sample Command

bash
# Generate GPG key
gpg --full-generate-key
git config --global user.signingkey <KEY_ID>
git config --global commit. gpgsign true

Task 5 – Resolve Merge Conflict
5.1 Demo Script

bash
# Simulate conflict
git checkout develop
echo "console.log('hello')" > app.js
git commit -am "Add log"
git checkout -b feature/bug
echo "console.log('world')" >> app.js
git commit -am "Add world"
git checkout develop
echo "console.log('hi')" >> app.js
git commit -am "Add hi"
git merge feature/bug  # CONFLICT!

# Resolve
git status          Shows conflict in app.js
# Manually edit app.js → keep desired lines
git add app.js
git commit          # Merge commit

# Prevent future
- **Rebase before PR**: `git pull --rebase origin develop`
- **Short-lived branches**: merge within 24h.
- **Protected branches**: enforce CI + PR.


5.2 Prevention Checklist
- Rebase workflow for feature branches.
- Small PRs (max 300 lines).
- Daily standup to flag overlapping files.
