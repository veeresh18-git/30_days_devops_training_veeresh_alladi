# 1. Git Fork vs Git Clone

### Git Clone

Creates a **local copy** of a remote repo.

```bash
git clone https://github.com/org/project.git
```

Used when:

* I’m a contributor in the same repo.
* Daily internal team work.

---

### Git Fork

Creates **my own copy** of someone else’s repo on [GitHub](https://github.com/?utm_source=chatgpt.com).

Flow:

```text
Original Repo -> Fork (my account) -> Clone locally
```

Used when:

* contributing to open source
* no direct write access

---

# 2. Scenario where you used Fork instead of Clone

“I contributed to [Kubernetes](https://github.com/kubernetes/kubernetes?utm_source=chatgpt.com) documentation once. I didn’t have write access to upstream, so I:

1. Forked repo to my GitHub
2. Cloned my fork locally
3. Created branch
4. Made changes
5. Raised Pull Request to upstream”

That’s standard open-source workflow.

---

# 3. Git Fetch vs Git Pull

### Fetch

Downloads latest changes **without merging**.

```bash
git fetch origin
```

Safe.

---

### Pull

Fetch + Merge

```bash
git pull origin main
```

Auto-merges.

---

# 4. Real-time example

Developer A pushes code.

On my machine:

```bash
git fetch
git diff main origin/main
```

See changes safely.

Then:

```bash
git merge origin/main
```

OR

```bash
git pull
```

Does both together.

---

# 5. Which do you use more?

**Answer: Fetch.**

“In production repos, I prefer `git fetch` because it gives me control before merging. It avoids accidental merge issues.”

Strong interview answer.

---

# 6. Git Rebase vs Git Merge

### Merge

Keeps history.

```text
A---B---C
     \   \
      D---M
```

Command:

```bash
git merge feature
```

---

### Rebase

Linear history.

```text
A---B---C---D
```

Command:

```bash
git rebase main
```

---

# 7. Short interview answer

“Merge preserves history; rebase rewrites history to keep it clean.
I use rebase before PRs and merge for shared branches.”

---

# 8. Git branching strategy used in your org

“We follow **GitFlow-lite**.”

Branches:

* `main` → production
* `develop` → integration
* `feature/*`
* `release/*`
* `hotfix/*`

Example:

```text
main
 └── develop
      └── feature/payment-api
```

For smaller teams, trunk-based also works.

---

# 9. Challenges faced with Git

Common:

* merge conflicts
* accidental force push
* secret leaks
* detached HEAD
* wrong branch commits
* large binary files
* rebase confusion

---

# 10. Recent Git issue and solution

“One engineer force-pushed to `main`, overwriting commits.

I recovered using:

```bash
git reflog
git reset --hard <commit>
git push --force
```

Then enabled branch protection in [GitHub Branch Protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches?utm_source=chatgpt.com).”

Good real-world answer.

---

# 11. How do you handle merge conflicts?

Steps:

```bash
git pull
```

Conflict appears.

Check:

```bash
git status
```

Open file:

```text
<<<<<<< HEAD
my code
=======
their code
>>>>>>> branch
```

Resolve manually.

Then:

```bash
git add .
git commit
```

---

# 12. Ours vs Theirs

Keep mine:

```bash
git checkout --ours file.txt
```

Keep theirs:

```bash
git checkout --theirs file.txt
```

Use case:

* config files
* generated files

---

# 13. Have you used Git tags?

Yes.

Used for releases:

```bash
git tag v1.0
git push origin v1.0
```

Example:

* rollback to old release
* deployment versions

CI/CD often deploys by tag.

---

# 14. Combine multiple commits into one

Interactive rebase:

```bash
git rebase -i HEAD~3
```

Change:

```text
pick
squash
squash
```

Result: one clean commit.

Used before PR.

---

# 15. 10 Git commands used daily

```bash
git clone
git status
git add .
git commit -m
git push
git pull
git fetch
git checkout
git branch
git log --oneline
git diff
git stash
```

---

# 16. Ignore a file

Use `.gitignore`

Example:

```text
*.log
.env
node_modules/
.pem
```

Then:

```bash
git add .gitignore
```

Important for secrets.

---

# 17. Purpose of `.git` folder

Stores repo metadata:

* commits
* branches
* tags
* history
* config
* objects

Without it, folder becomes normal files.

---

# 18. Can you restore deleted `.git` folder?

If deleted:

* easiest: reclone

```bash
git clone repo_url
```

If local changes:
copy files to new clone.

Sometimes:

```bash
git init
git remote add origin ...
```

but history is lost.

---

# 19. Secret leaked in Git — what do you do?

Example: AWS key leaked.

Immediate:

1. revoke secret in [AWS IAM](https://console.aws.amazon.com/iam/?utm_source=chatgpt.com)
2. rotate key

Then remove from history:

Using [git-filter-repo](https://github.com/newren/git-filter-repo?utm_source=chatgpt.com):

```bash
git filter-repo --path secret.txt --invert-paths
```

Force push:

```bash
git push --force
```

Prevent:

* pre-commit hooks
* [GitHub Secret Scanning](https://docs.github.com/en/code-security/secret-scanning/about-secret-scanning?utm_source=chatgpt.com)
* vault tools like [HashiCorp Vault](https://www.vaultproject.io/?utm_source=chatgpt.com)

**Interview line:**
“Rotation is priority one; removing from Git history is step two.”

