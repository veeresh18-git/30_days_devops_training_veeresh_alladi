# 30_days_devops_training_veeresh_alladi

GIT:
----------
Here’s a hands-on example for each Git concept, explaining why it is used, what problem it solves, and how to implement it. Each example is designed to be practical and easy to follow.

1. Branching
Why it is used?
Branching allows developers to work on different features, bug fixes, or experiments in isolation without affecting the main codebase. It solves the problem of conflicting changes when multiple developers work on the same project.
Problem it solves:

Avoids overwriting changes in the main branch.
Enables parallel development.

Hands-on Example:

Create a new branch for a feature:
git branch feature-login


Switch to the new branch:
git checkout feature-login


Make changes to your code (e.g., add a new login feature).
Commit the changes:
git add .
git commit -m "Added login feature"


Merge the branch back into the main branch:
git checkout main
git merge feature-login




2. Merging and Merge Conflicts
Why it is used?
Merging combines changes from one branch into another. Merge conflicts occur when changes in two branches overlap, and Git cannot automatically resolve them.
Problem it solves:

Integrates changes from multiple branches.
Resolves conflicts when two developers edit the same file.

Hands-on Example:

Simulate a merge conflict:

On main branch, edit file.txt and commit:
echo "Main branch change" >> file.txt
git add file.txt
git commit -m "Change in main branch"


On feature-login branch, edit the same line in file.txt and commit:
echo "Feature branch change" >> file.txt
git add file.txt
git commit -m "Change in feature branch"




Merge feature-login into main:
git checkout main
git merge feature-login


Resolve the conflict manually in file.txt:
<<<<<<< HEAD
Main branch change
=======
Feature branch change
>>>>>>> feature-login

Edit the file to keep the desired content, then:
git add file.txt
git commit -m "Resolved merge conflict"




3. Git Reset
Why it is used?
git reset is used to undo changes in your repository. It solves the problem of accidental commits or staging the wrong files.
Problem it solves:

Undo commits or changes safely.
Revert to a previous state.

Hands-on Example:

Make a commit:
echo "Test content" >> file.txt
git add file.txt
git commit -m "Added test content"


Undo the commit but keep changes staged:
git reset --soft HEAD~1


Undo the commit and unstage changes:
git reset --mixed HEAD~1



Undo the commit and discard changes:
git reset --hard HEAD~1




4. Git Rebase
Why it is used?
Rebasing rewrites commit history to create a linear sequence of commits. It solves the problem of messy commit history and makes it easier to review changes.
Problem it solves:

Keeps a clean commit history.
Avoids unnecessary merge commits.

Hands-on Example:

Create a feature branch and make commits:
git checkout -b feature-login
echo "Login feature" >> file.txt
git add file.txt
git commit -m "Added login feature"


Rebase onto the main branch:
git checkout main
git pull
git checkout feature-login
git rebase main


Resolve conflicts if any, then continue:
git rebase --continue




5. Cherry-Picking
Why it is used?
Cherry-picking applies specific commits from one branch to another. It solves the problem of needing only certain changes without merging the entire branch.
Problem it solves:

Selectively apply changes.
Avoid merging unrelated commits.

Hands-on Example:

Identify the commit hash you want to cherry-pick:
git log --oneline


Apply the commit to another branch:
git checkout main
git cherry-pick <commit-hash>




6. Stashing Changes
Why it is used?
Stashing saves uncommitted changes temporarily. It solves the problem of switching branches without committing incomplete work.
Problem it solves:

Temporarily save work in progress.
Avoid committing incomplete changes.

Hands-on Example:

Make changes to a file but don’t commit:
echo "Work in progress" >> file.txt


Stash the changes:
git stash


Switch branches and work on something else:
git checkout main


Apply the stashed changes:
git stash apply




7. Viewing Commit History
Why it is used?
Viewing commit history helps track changes and understand the evolution of the codebase. It solves the problem of identifying when and why changes were made.
Problem it solves:

Tracks changes over time.
Identifies specific commits.

Hands-on Example:

View detailed commit history:
git log


View a concise history:
git log --oneline


View changes introduced by a specific commit:
git show <commit-hash>




8. Branching Strategies
Why it is used?
Branching strategies like Git Flow or Feature Branching organize development workflows. They solve the problem of managing multiple features, releases, and bug fixes simultaneously.
Problem it solves:

Organizes team workflows.
Ensures stable releases.

Hands-on Example:

Use Git Flow:

Create a develop branch:
git branch develop
git checkout develop


Create feature branches from develop:
git checkout -b feature-login


Merge features into develop and main for releases.




Summary
Each Git concept addresses a specific problem in version control, from managing parallel development (branching) to undoing mistakes (reset) and keeping a clean history (rebase). By practicing these hands-on examples, you’ll gain a deeper understanding of how Git works and how to use it effectively in real-world projects.
