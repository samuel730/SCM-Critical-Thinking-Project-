# Source-Code-Management-Project-3

Task 1: Evaluate Different SCM Tools

Report: Why Git is the Best Choice for Our Team

After researching and evaluating different SCM tools, I strongly recommend transitioning to Git, a distributed version control system (DVCS). Here's why:

- Flexibility and Scalability: Git allows developers to work offline, and its distributed nature ensures that every developer has a full copy of the repository, making it easier to manage large codebases.
- Collaboration: Git's branching model enables multiple developers to work on different features simultaneously without conflicts, making it ideal for distributed teams.
- Security: Git's use of SHA-1 hashes ensures data integrity, and its support for signed commits and tags provides an additional layer of security.
- Community Support: Git is widely used in the industry, and its large community ensures that there are plenty of resources available for learning and troubleshooting.

While other DVCS tools like Mercurial are available, Git's widespread adoption and extensive feature set make it the best choice for our team.

Task 2: Implement Git Workflows for a Team Project

Git Workflow Guide

Our Git workflow will follow the Git Flow model, which includes the following branches:

- main: The main branch will contain the production-ready code.
- develop: The develop branch will serve as the main branch for development, where feature branches will be merged.
- feature: Feature branches will be used for developing new features.

Branching Strategy

1. Create a new feature branch from the develop branch for each new feature.
2. Use a descriptive name for the feature branch (e.g., feature/login-system).
3. Merge the feature branch into the develop branch once it's complete.

Pull Requests

1. Create a pull request for each feature branch to ensure code review.
2. Assign a reviewer to each pull request.
3. Ensure that the pull request is approved before merging it into the develop branch.

Best Practices

- Use meaningful commit messages.
- Keep commits small and focused on one change.
- Use git rebase to squash commits before merging.

Task 3: Automate Code Quality and Deployment

.github/workflows/ci-cd.yml


name: CI/CD Pipeline

on:
  push:
    branches:
      - develop

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run unit tests
        run: |
          npm install
          npm test

      - name: Check code quality
        run: |
          npm run lint

      - name: Deploy to staging
        uses: deploy-to-staging@v1
        env:
          STAGING_API_KEY: ${{ secrets.STAGING_API_KEY }}


This pipeline will run unit tests, check code quality, and deploy changes to a staging environment when code is pushed to the develop branch.

Task 4: Enforce Security Best Practices

Security Best Practices

1. User Access Controls: Use GitHub teams to manage access to the repository.
2. SSH Keys: Use SSH keys to authenticate with the repository.
3. Branch Protection: Enable branch protection for the main branch to prevent unauthorized changes.
4. Signed Commits: Use signed commits for critical code changes.

Configuring Security Best Practices

1. Create a GitHub team and add members to it.
2. Generate SSH keys and add them to the repository.
3. Enable branch protection for the main branch.
4. Configure signed commits using GPG keys.

Task 5: Handle a Real-World Git Challenge

Resolving Merge Conflicts

1. Pull the latest code: git pull origin main
2. Identify conflicting files: git status
3. Merge changes: git merge feature/login-system
4. Resolve conflicts manually.
5. Commit the resolved changes: git commit -m "Resolved merge conflict"

Preventing Future Conflicts

1. Communicate with team members about changes.
2. Use smaller, more frequent pull requests.
3. Use git rebase to squash commits before merging.

By following these best practices and workflows, we can ensure a smooth transition to Git and improve our overall development process.
