# Git Workflow for Feature Branching

- **Main/Master Branch**:  
  The `main` (or `master`) branch points to production. No direct changes are allowed on this branch to ensure stability.

- **Feature Branches**:  
  In a project where multiple team members are involved, instead of making changes directly to the `main` branch, each developer should create a **feature branch** (a copy of the `main` branch). This allows them to work independently on their own changes.

- **Independent Development**:  
  Developers can work on their own feature branches without affecting the work of others. These branches are isolated environments where developers implement and test their changes.

- **Code Reviews**:  
  Once changes are completed on the feature branch, developers should request reviews from their peers by creating a **pull request (PR)**. This ensures that changes are reviewed for quality, bugs, and adherence to project standards.

- **Merging to Main**:  
  After receiving approval, the feature branch is merged into the `main` branch. This integrates the changes into the production-ready codebase.



---
$ git log --oneline
aa6a967 (HEAD -> main, origin/main) added ...
bc2c239 added terraform info
fe0f59e added ansibel Push and Pull info
16f247d added few more directories
093fc28 Add explanation of Linux filesystem structure

every commit should have one key i.e ==> sha code (40 chars) and value is changes


$ git cat-file bc2c239 -p  
tree 89933f95b84e664f61a20adac7add3b269c9c1a9
parent b077e6f3e4f4bf3802a0cdfbcb2b3ea710e652e2
author vignesh <vigneshnalla777@gmail.com> 1729647244 +0530
committer vignesh <vigneshnalla777@gmail.com> 1729647244 +0530


### Unpushed Commits in Git

```bash
git log origin/main..HEAD
```
This command shows all the commits that are in your local branch (HEAD refers to your current branch, typically main) but not in the remote branch (origin/main). If you have committed changes but haven't pushed them to the remote, you'll see those commits listed here.

### Branch
```bash
git checkout -b <Branch-name>
```
it create a new branch and both new and main branch will point to the same hash code becuase we have not made any changes 


pull request:
when you want to raise a pull request before anyone merging code into main branch you need to add classic branch protection rule in setting/branch in that add main branch and prtect merging branches[ select require a pull request before merging]