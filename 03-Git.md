# 🔀 Git & GitHub Interview Questions & Answers

This file contains **50 Git and GitHub interview questions and answers** for DevOps Engineer interview preparation.

* **20 Normal Questions & Answers**
* **30 Scenario-Based Questions & Answers**

---

# 📚 Part 1 — Normal Git Interview Questions

## Q1. What is Git?

**Answer:**

Git is a **distributed version control system (VCS)** used to track changes in source code and collaborate with other developers.

Git allows us to:

* Track code changes
* Create branches
* Merge code
* Revert changes
* Collaborate with developers
* Maintain different versions of code

Basic workflow:

```text
Working Directory
       ↓
Staging Area
       ↓
Local Repository
       ↓
Remote Repository
```

---

## Q2. What is GitHub?

**Answer:**

GitHub is a cloud-based platform for hosting and collaborating on Git repositories.

Git is the version-control tool, while GitHub provides a platform for:

* Remote repositories
* Pull Requests
* Code reviews
* Issues
* Actions
* Collaboration
* Repository management

Example:

```text
Developer
    ↓
   Git
    ↓
GitHub Repository
```

---

## Q3. What is the difference between Git and GitHub?

**Answer:**

| Git                     | GitHub                             |
| ----------------------- | ---------------------------------- |
| Version control system  | Git hosting/collaboration platform |
| Runs locally            | Cloud/web platform                 |
| Tracks code changes     | Hosts Git repositories             |
| Command-line tool       | Web UI + APIs + Git hosting        |
| Can work without GitHub | Commonly uses Git repositories     |

Example:

```bash
git init
git add .
git commit
```

These are Git operations.

Pushing to a GitHub repository:

```bash
git push origin main
```

uses Git to transfer changes to GitHub.

---

## Q4. What is a Git repository?

**Answer:**

A Git repository is a directory containing the project's files and Git's version-control information.

A repository can be:

```text
Local Repository
```

or:

```text
Remote Repository
```

Example:

```text
project/
├── app.py
├── Dockerfile
├── README.md
└── .git/
```

The `.git` directory contains Git metadata.

---

## Q5. What is `git init`?

**Answer:**

`git init` initializes a new Git repository in the current directory.

Example:

```bash
git init
```

This creates a `.git` directory.

After initialization:

```text
Project Directory
       ↓
    git init
       ↓
Git Repository
```

---

## Q6. What is `git clone`?

**Answer:**

`git clone` creates a local copy of a remote Git repository.

Example:

```bash
git clone https://github.com/user/project.git
```

It downloads:

* Project files
* Git history
* Branch information
* Repository metadata

---

## Q7. What is `git status`?

**Answer:**

`git status` shows the current state of the working directory and staging area.

Example:

```bash
git status
```

It can show:

* Modified files
* Untracked files
* Staged files
* Current branch
* Changes ready to commit

It is one of the most useful Git commands during daily development.

---

## Q8. What is the difference between `git add`, `git commit`, and `git push`?

**Answer:**

### `git add`

Moves changes to the staging area.

```bash
git add file.txt
```

### `git commit`

Saves staged changes to the local repository.

```bash
git commit -m "Add new feature"
```

### `git push`

Uploads local commits to the remote repository.

```bash
git push origin main
```

Workflow:

```text
File Change
    ↓
git add
    ↓
Staging Area
    ↓
git commit
    ↓
Local Repository
    ↓
git push
    ↓
GitHub
```

---

## Q9. What is a Git branch?

**Answer:**

A branch is an independent line of development.

For example:

```text
main
 │
 ├── feature/login
 ├── feature/payment
 └── bugfix/database
```

Branches allow developers to work on features without directly modifying the main branch.

Create a branch:

```bash
git branch feature/login
```

Create and switch:

```bash
git switch -c feature/login
```

---

## Q10. What is the difference between `git merge` and `git rebase`?

**Answer:**

`git merge` combines two branches and generally creates a merge commit when needed.

```bash
git switch main
git merge feature
```

`git rebase` moves your commits onto a new base.

```bash
git switch feature
git rebase main
```

Simplified:

```text
Merge:

A---B---C---M
     \     /
      D---E


Rebase:

A---B---C---D'---E'
```

Merge preserves the branching history, while rebase creates a more linear history.

Avoid rebasing commits that other people are already depending on unless the team has agreed to do so.

---

## Q11. What is `git pull`?

**Answer:**

`git pull` fetches changes from a remote repository and integrates them into the current branch.

Conceptually:

```text
git pull
    =
git fetch
+
integration step
```

Example:

```bash
git pull origin main
```

---

## Q12. What is the difference between `git fetch` and `git pull`?

**Answer:**

`git fetch` downloads remote changes without integrating them into the current branch.

```bash
git fetch origin
```

`git pull` downloads and integrates the changes.

```bash
git pull origin main
```

A common safe workflow for reviewing changes is:

```bash
git fetch origin
git log HEAD..origin/main
```

before deciding how to integrate them.

---

## Q13. What is `git stash`?

**Answer:**

`git stash` temporarily stores uncommitted changes so that you can work on another task.

Example:

```bash
git stash
```

Later:

```bash
git stash pop
```

You can list stashes:

```bash
git stash list
```

Useful scenario:

```text
Working on Feature A
       ↓
Urgent bug appears
       ↓
git stash
       ↓
Fix bug
       ↓
Return to Feature A
       ↓
git stash pop
```

---

## Q14. What is `.gitignore`?

**Answer:**

`.gitignore` specifies files and directories that Git should not track.

Example:

```text
node_modules/
.env
*.log
.terraform/
__pycache__/
```

Example `.gitignore`:

```gitignore
node_modules/
.env
*.log
```

It is commonly used to prevent temporary files, secrets, dependencies, and generated files from being committed.

---

## Q15. What is a Git commit?

**Answer:**

A commit is a snapshot of staged changes stored in the Git repository history.

Example:

```bash
git add .
git commit -m "Add Docker configuration"
```

A commit contains information such as:

* Author
* Timestamp
* Commit message
* Parent commit
* Snapshot/reference to project content

---

## Q16. What is `git log`?

**Answer:**

`git log` displays commit history.

Basic:

```bash
git log
```

Compact format:

```bash
git log --oneline
```

Graph view:

```bash
git log --oneline --graph --all
```

This is useful for understanding project history and troubleshooting changes.

---

## Q17. What is `git revert`?

**Answer:**

`git revert` creates a **new commit that reverses the changes introduced by an earlier commit**.

Example:

```bash
git revert <commit-id>
```

It is generally safer for shared branches because it does not rewrite existing history.

Example:

```text
A → B → C
        ↓
      revert C
        ↓
A → B → C → D
```

---

## Q18. What is `git reset`?

**Answer:**

`git reset` moves the current branch reference and can also modify the staging area and working tree depending on the option used.

Common forms:

```bash
git reset --soft HEAD~1
```

Moves the branch back while keeping changes staged.

```bash
git reset --mixed HEAD~1
```

Moves the branch back and unstages changes while keeping them in the working directory.

```bash
git reset --hard HEAD~1
```

Moves the branch back and discards tracked working-tree changes associated with the reset.

`--hard` should be used carefully.

---

## Q19. What is a Pull Request?

**Answer:**

A Pull Request (PR) is a request to merge changes from one branch into another branch, commonly on GitHub.

Example:

```text
feature/login
      ↓
 Pull Request
      ↓
 code review
      ↓
 tests
      ↓
 merge
      ↓
main
```

PRs commonly provide:

* Code review
* Automated testing
* Discussion
* Approval
* Change tracking

---

## Q20. What is a Git tag?

**Answer:**

A Git tag is a reference to a specific commit, commonly used to mark releases.

Example:

```bash
git tag v1.0.0
```

Push the tag:

```bash
git push origin v1.0.0
```

Tags are useful for identifying versions such as:

```text
v1.0.0
v1.1.0
v2.0.0
```

---

# 🔥 Part 2 — 30 Scenario-Based Git & GitHub Interview Questions

## Q21. Scenario: `git push` is rejected. How will you troubleshoot it?

**Answer:**

First I would read the exact error message.

Common causes include:

* Authentication failure
* Permission denied
* Remote branch has new commits
* Branch protection
* Wrong remote URL

Check the remote:

```bash
git remote -v
```

Check the current branch:

```bash
git branch --show-current
```

Check repository status:

```bash
git status
```

If the remote branch contains commits that I don't have:

```bash
git fetch origin
```

Then inspect the differences and integrate the changes appropriately.

I would not use `git push --force` immediately.

---

## Q22. Scenario: You get a GitHub authentication error while pushing. What will you check?

**Answer:**

First check the remote URL:

```bash
git remote -v
```

For HTTPS, GitHub does not support using an account password as Git authentication. I would use an appropriate authentication method such as:

* Personal Access Token
* SSH authentication

For SSH, test:

```bash
ssh -T git@github.com
```

I would also verify that the authenticated account has permission to the repository.

---

## Q23. Scenario: You accidentally committed an `.env` file containing credentials. What will you do?

**Answer:**

I would treat the credentials as compromised.

### Step 1

Immediately rotate/revoke the exposed credentials.

### Step 2

Remove the file from tracking:

```bash
git rm --cached .env
```

Add it to `.gitignore`:

```gitignore
.env
```

### Step 3

If the secret exists in Git history, removing it only from the latest commit is not enough.

I would use an appropriate history-rewriting tool/process to remove the secret from repository history.

### Step 4

Check for unauthorized usage.

The key point is:

```text
Remove secret ≠ Secret is safe
```

The exposed credential should be rotated.

---

## Q24. Scenario: You have uncommitted changes, but you need to switch to another branch urgently. What will you do?

**Answer:**

I can temporarily save the changes:

```bash
git stash
```

Switch branches:

```bash
git switch bugfix
```

After completing the urgent work, return:

```bash
git switch feature
```

Restore changes:

```bash
git stash pop
```

---

## Q25. Scenario: Two developers modified the same lines and now there is a merge conflict. How will you solve it?

**Answer:**

First identify conflicted files:

```bash
git status
```

Open the conflicted files.

Git may show:

```text
<<<<<<< HEAD
Current branch
=======
Incoming changes
>>>>>>> feature
```

I would:

1. Understand both changes.
2. Decide which code should remain.
3. Remove conflict markers.
4. Test the code.
5. Stage the resolved file.

```bash
git add file.txt
```

For a merge:

```bash
git commit
```

For a rebase:

```bash
git rebase --continue
```

---

## Q26. Scenario: You committed the wrong file but have not pushed it yet. What will you do?

**Answer:**

If I want to modify the latest commit, I can remove the file from the commit and amend it.

For example:

```bash
git reset HEAD~1
```

Then stage only the correct files:

```bash
git add correct-file.txt
```

and commit again:

```bash
git commit -m "Correct commit"
```

Another approach is to amend the latest commit if only small changes are needed.

Because the commit has not been pushed, rewriting this local history is generally straightforward.

---

## Q27. Scenario: You pushed a bad commit to a shared production branch. What will you do?

**Answer:**

I would avoid rewriting shared history unless the team explicitly requires it.

Instead:

```bash
git revert <commit-id>
```

Then:

```bash
git push origin main
```

This creates a new commit that reverses the problematic change.

I would then investigate why the bad change reached production and improve the review/CI process.

---

## Q28. Scenario: You accidentally deleted a local branch. Can you recover it?

**Answer:**

Often yes, if the relevant commit still exists in the repository's reflog.

Check:

```bash
git reflog
```

Find the commit associated with the deleted branch.

Then recreate the branch:

```bash
git switch -c feature-branch <commit-id>
```

This is one reason `git reflog` is an important troubleshooting command.

---

## Q29. Scenario: You want to see what changed between your local branch and the remote branch. What will you do?

**Answer:**

First update remote references:

```bash
git fetch origin
```

Then compare:

```bash
git diff HEAD origin/main
```

To see commits that exist remotely but not locally:

```bash
git log HEAD..origin/main --oneline
```

---

## Q30. Scenario: A developer says their local branch is behind `main`. How would you update it?

**Answer:**

First fetch the latest changes:

```bash
git fetch origin
```

Then I could merge:

```bash
git switch feature
git merge origin/main
```

Or, if the team prefers a linear history and the branch is appropriate to rebase:

```bash
git switch feature
git rebase origin/main
```

I would follow the team's branching policy.

---

## Q31. Scenario: You need to undo the last commit but keep all the changes. What command will you use?

**Answer:**

Use:

```bash
git reset --soft HEAD~1
```

This moves the branch pointer back one commit while keeping the changes staged.

If I want to keep the changes but unstage them:

```bash
git reset --mixed HEAD~1
```

---

## Q32. Scenario: You need to completely discard the latest local commit and its tracked changes. What will you use?

**Answer:**

If I am certain the changes should be discarded:

```bash
git reset --hard HEAD~1
```

This should be used carefully because tracked working-tree changes can be lost.

Before using it, I would confirm that the changes are not required.

---

## Q33. Scenario: You committed a change locally but want to modify the commit message. What will you do?

**Answer:**

For the latest commit:

```bash
git commit --amend -m "Correct commit message"
```

If the commit has already been pushed to a shared branch, I would avoid rewriting history unless the team has explicitly agreed.

---

## Q34. Scenario: You need to find who changed a particular line of code. Which command will you use?

**Answer:**

Use:

```bash
git blame filename
```

It can show:

* Commit
* Author
* Date
* Line

Example:

```bash
git blame app.py
```

Then I can inspect the corresponding commit:

```bash
git show <commit-id>
```

---

## Q35. Scenario: You need to find which commit introduced a bug. How can Git help?

**Answer:**

One useful approach is **`git bisect`**.

Start:

```bash
git bisect start
```

Mark the current bad commit:

```bash
git bisect bad
```

Mark a known-good commit:

```bash
git bisect good <commit-id>
```

Git checks out a commit between them.

I test it and mark:

```bash
git bisect good
```

or:

```bash
git bisect bad
```

Git repeats this process until it identifies the commit that introduced the bug.

---

## Q36. Scenario: A developer wants to download remote changes without changing their current branch. What command should they use?

**Answer:**

Use:

```bash
git fetch
```

This downloads remote references and objects without automatically integrating them into the current branch.

After fetching, I can inspect:

```bash
git log origin/main
```

or:

```bash
git diff HEAD origin/main
```

before deciding what to do.

---

## Q37. Scenario: You cloned a repository, but Git says there is no upstream branch when you push. What will you do?

**Answer:**

I would establish the upstream relationship:

```bash
git push -u origin main
```

After that, future pushes can usually be:

```bash
git push
```

I would first confirm the branch name:

```bash
git branch --show-current
```

---

## Q38. Scenario: You want to remove a file from Git tracking but keep it on your local machine. What will you do?

**Answer:**

Use:

```bash
git rm --cached filename
```

Then add it to `.gitignore`:

```gitignore
filename
```

Commit the change:

```bash
git add .gitignore
git commit -m "Stop tracking local file"
```

The file remains on the local filesystem but is no longer tracked.

---

## Q39. Scenario: Your repository contains a huge file and GitHub rejects the push. What will you do?

**Answer:**

First identify the large file.

```bash
git status
```

If it has not been committed, remove it before committing.

If it exists in Git history, simply deleting it from the latest commit may not be sufficient because the large object remains in history.

I would use an appropriate history-rewriting tool to remove it.

For legitimate large binary files, I would consider **Git Large File Storage (Git LFS)** where appropriate.

---

## Q40. Scenario: You want to create a release version of your application. How can Git help?

**Answer:**

I would create a Git tag pointing to the release commit.

For example:

```bash
git tag v1.0.0
```

Push it:

```bash
git push origin v1.0.0
```

This allows the team to identify exactly which commit corresponds to version `1.0.0`.

---

## Q41. Scenario: A developer accidentally committed generated files such as `node_modules`. How will you fix it?

**Answer:**

First add it to `.gitignore`:

```gitignore
node_modules/
```

Then remove it from Git tracking:

```bash
git rm -r --cached node_modules
```

Commit:

```bash
git add .gitignore
git commit -m "Ignore generated dependencies"
```

If the directory is present in old commits and repository size is a concern, I would consider cleaning the repository history.

---

## Q42. Scenario: Your local repository has many old branches. How will you identify branches that have already been merged?

**Answer:**

List merged branches:

```bash
git branch --merged
```

List branches not merged:

```bash
git branch --no-merged
```

After confirming that a local branch is no longer required:

```bash
git branch -d branch-name
```

I would be careful before deleting branches that may still contain useful work.

---

## Q43. Scenario: You need to compare two commits. What command will you use?

**Answer:**

Use:

```bash
git diff commit1 commit2
```

To compare commits at a higher level:

```bash
git log commit1..commit2 --oneline
```

To inspect a particular commit:

```bash
git show commit-id
```

---

## Q44. Scenario: A Git repository has become very large and slow. How will you troubleshoot it?

**Answer:**

I would investigate:

* Large files
* Large binary files
* Generated files
* Old objects
* Unnecessary history

I would inspect repository statistics and search for large objects.

Possible solutions include:

* `.gitignore`
* Git LFS
* History cleanup
* Removing generated artifacts
* Moving build artifacts to artifact storage

I would avoid deleting Git history without understanding the impact on all repository users.

---

## Q45. Scenario: A developer wants to temporarily switch to another branch without losing their current work. What will you recommend?

**Answer:**

If the current work is not ready for a commit:

```bash
git stash
```

Then:

```bash
git switch other-branch
```

After finishing:

```bash
git switch original-branch
git stash pop
```

If the work is meaningful and should be preserved permanently, creating a WIP commit on a private branch may be better than repeatedly stashing.

---

## Q46. Scenario: Your branch has three commits that should become one clean commit before a Pull Request. What can you use?

**Answer:**

Interactive rebase:

```bash
git rebase -i HEAD~3
```

I can change later commits from:

```text
pick
```

to:

```text
squash
```

or:

```text
fixup
```

This combines commits into a cleaner history.

I would only rewrite commits that are safe to rewrite and not already being relied upon by others.

---

## Q47. Scenario: A Pull Request passes locally but fails in CI/CD. How will you troubleshoot?

**Answer:**

I would compare the local environment with the CI environment.

I would check:

* Git commit/branch
* Build logs
* Environment variables
* Secrets
* Dependency versions
* Runtime version
* Operating system
* Network access
* Test configuration

I would first read the CI logs to identify the exact failure.

For example:

```text
GitHub
   ↓
Pull Request
   ↓
CI Pipeline
   ↓
Build
   ↓
Test
   ↓
Deploy
```

The goal is to identify whether the problem is code, configuration, dependency, or environment related.

---

## Q48. Scenario: Someone pushed directly to the `main` branch, bypassing the normal Pull Request process. How would you prevent this?

**Answer:**

I would configure GitHub branch protection/rules for `main`.

Depending on the team's policy, I would require:

* Pull Requests
* Required approvals
* Passing CI checks
* Code review
* Restricted direct pushes

A typical workflow becomes:

```text
Developer
    ↓
Feature Branch
    ↓
Pull Request
    ↓
Code Review
    ↓
CI Tests
    ↓
Approval
    ↓
main
```

---

## Q49. Scenario: Your Git working directory contains changes, and you want to know exactly what has changed before committing. What commands will you use?

**Answer:**

Check status:

```bash
git status
```

View unstaged changes:

```bash
git diff
```

View staged changes:

```bash
git diff --cached
```

Then I would stage only the intended files:

```bash
git add file1 file2
```

and review the staged diff again:

```bash
git diff --cached
```

This helps prevent accidental changes from entering the commit.

---

## Q50. Scenario: Explain a production-ready Git workflow for a DevOps team.

**Answer:**

I would use a controlled branch and CI/CD workflow.

Example:

```text
                     GitHub
                       │
             ┌─────────┴─────────┐
             │                   │
        Feature Branch       Bugfix Branch
             │                   │
             └─────────┬─────────┘
                       ↓
                 Pull Request
                       ↓
                 Code Review
                       ↓
                    CI Tests
                       ↓
                Security Checks
                       ↓
                    Approval
                       ↓
                    main
                       ↓
                 Build Artifact
                       ↓
                  Deployment
                       ↓
                 Production
```

### Recommended practices

**1. Protect `main`**

Do not allow uncontrolled direct pushes.

**2. Use feature branches**

Example:

```text
feature/login
feature/payment
bugfix/database
```

**3. Use Pull Requests**

All production changes should go through review.

**4. Automate testing**

Run tests automatically for Pull Requests.

**5. Use meaningful commits**

Example:

```bash
git commit -m "Add health check endpoint"
```

instead of:

```bash
git commit -m "changes"
```

**6. Never commit secrets**

Use:

```text
Secrets Manager
CI/CD credentials
Environment-specific secret stores
```

instead of committing passwords or API keys.

**7. Tag releases**

Example:

```bash
git tag v1.0.0
```

**8. Use rollback mechanisms**

For a bad production deployment, revert the change or deploy a previously verified version.

---

# 🎯 Git Troubleshooting Cheat Sheet

| Problem                     | Useful Commands            |
| --------------------------- | -------------------------- |
| Check repository status     | `git status`               |
| View history                | `git log --oneline`        |
| View changes                | `git diff`                 |
| View staged changes         | `git diff --cached`        |
| Check remote                | `git remote -v`            |
| Download remote changes     | `git fetch`                |
| Update current branch       | `git pull`                 |
| Save temporary changes      | `git stash`                |
| Recover deleted branch      | `git reflog`               |
| Find who changed a line     | `git blame`                |
| Find bug-introducing commit | `git bisect`               |
| Undo shared commit          | `git revert`               |
| Undo local commit           | `git reset`                |
| Create branch               | `git switch -c branch`     |
| Merge branch                | `git merge branch`         |
| Rebase branch               | `git rebase branch`        |
| Compare commits             | `git diff commit1 commit2` |
| Create release tag          | `git tag v1.0.0`           |

---

# 🧠 Git Interview Troubleshooting Framework

When an interviewer gives you a Git scenario:

```text
1. Read the exact error
          ↓
2. Check git status
          ↓
3. Check current branch
          ↓
4. Check remote configuration
          ↓
5. Inspect history/differences
          ↓
6. Identify the root cause
          ↓
7. Apply the safest solution
          ↓
8. Test/verify
          ↓
9. Push or merge according to team policy
```

**Important:** In a production/shared repository, avoid destructive commands such as:

```bash
git push --force
git reset --hard
```

unless you understand the consequences and have authorization to use them.

---

# 🎤 Important Git Interview Topics

Before an interview, make sure you can explain:

* [ ] Git vs GitHub
* [ ] Repository
* [ ] Working directory
* [ ] Staging area
* [ ] Commit
* [ ] Branch
* [ ] Merge
* [ ] Rebase
* [ ] Pull
* [ ] Fetch
* [ ] Push
* [ ] Stash
* [ ] Revert
* [ ] Reset
* [ ] Reflog
* [ ] Cherry-pick
* [ ] Git tags
* [ ] Git hooks
* [ ] `.gitignore`
* [ ] Pull Requests
* [ ] Merge conflicts
* [ ] Branch protection
* [ ] Git authentication
* [ ] GitHub workflows
* [ ] CI/CD integration
* [ ] Secret management
* [ ] Git troubleshooting

---

# 📊 Git Interview Preparation

```text
20 Normal Questions
        +
30 Scenario-Based Questions
        =
50 Git & GitHub Interview Questions
```

