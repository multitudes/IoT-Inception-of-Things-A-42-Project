# Git worktrees explained

This is useful to see what are the other team member working on since it saves different branches on separate folders of the repo.
`git worktree` lets you manage multiple working directories attached to a single Git repository. This is useful for working on different branches or features at the same time without switching branches in your main directory.

### How to use `git worktree`

#### 1. Add a new worktree
```sh
git worktree add ../feature-branch feature-branch
```
- This creates a new directory (`../feature-branch`) and checks out the branch `feature-branch` there.

#### 2. List all worktrees
```sh
git worktree list
```

#### 3. Remove a worktree
```sh
git worktree remove ../feature-branch
```

### Does it affect the origin?
No, using `git worktree` only affects your local machine. It does **not** change anything on the remote (`origin`) until you push commits from your worktree.

ou can run git pull from inside any worktree folder to update that branch from the remote.

How to select a worktree:
Just cd into the folder of the worktree you want to work on. For example:

```
cd ../feature-branch
git pull
```
Each worktree is a separate directory with its own checked-out branch. All normal Git commands (commit, push, pull, etc.) work inside that folder and affect only that branch.

Summary:

cd into the worktree directory you want.
Run your Git commands as usual.
Each worktree is independent and tied to its branch.