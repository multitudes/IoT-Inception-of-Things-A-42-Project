# How to add submodules in git

When cloning your main repo, use:
```sh
git clone --recurse-submodules https://github.com/yourusername/iot-hihi.git
```
to get all submodules.

Yes, you can have a **GitHub repository inside another GitHub repository** by using a feature called a **git submodule**.

### How to do it

If you are inside your main `iot-hihi` git repository, you can add `p3` as a submodule like this:

```sh
git submodule add https://github.com/multitudes/lbrusa42-IoT-Playground.git p3
git submodule update --init --recursive
```

This will create a `p3` directory in your repo, linked to the external GitHub repository.  
You can then commit and push the submodule reference to your main repo.

**To sync/update the submodule:**
```sh
git submodule update --remote p3
```

## how to commit and push
To **push changes to the submodule (`p3`)** itself, you must:

1. **Go into the `p3` directory:**
   ```sh
   cd p3
   ```

2. **Commit and push as usual:**
   ```sh
   git add .
   git commit -m "Your changes"
   git push
   ```

3. **Go back to your main repo (`iot-hihi`):**
   ```sh
   cd ..
   git add p3
   git commit -m "Update p3 submodule reference"
   git push
   ```

- `git submodule update --remote p3` is used to **pull updates** from the remote submodule into your main repo, not to push changes.
- You still need to use `git commit` and `git push` in both the submodule and the main repo to sync changes.

**Summary:**  
- Make changes in `p3`, commit and push inside `p3`.
- Update the submodule reference in your main repo, then commit and push in the main repo.
- `git submodule update --remote p3` is for pulling, not pushing.


You **can** go into `p3` and run `git pull` to update the submodule itself, but this only updates the submodule's local copy.  
To update the reference in your main repo (`iot-hihi`), you must:

1. `cd p3 && git pull` (updates submodule content)
2. `cd .. && git add p3 && git commit -m "Update p3 submodule reference" && git push` (updates main repo to point to the new commit in the submodule)

**Summary:**  
- `git pull` in `p3` updates the submodule, but you still need to commit and push the updated reference in your main repo.

**Summary:**  
- You can safely add `p3` as a submodule inside `iot-hihi`.
- This is the recommended way to include another repo inside your main project and keep it in sync with GitHub.
### Important notes

- Submodules are the recommended way to nest repos.
- You cannot simply `git init` inside a folder of an existing repo and push it to GitHub; use submodules instead.

**Summary:**  
Use git submodules for a repo-inside-a-repo setup.  
Direct nesting without submodules is not supported by git/GitHub.


## what if i make a mistake

i mistakenly did 
```
git submodule add https://github.com/multitudes/lbrusa42-IoT-Playground.git p3/argo
```
and wanted to remove and clone again as just p3... but i got error `fatal: 'p3' already exists in the index` because the `p3` directory is still tracked by git as a submodule or regular folder.

**To fully remove and re-add the submodule:**

1. **Remove the submodule from the index:**
   ```sh
   git rm -f p3
   ```

2. **Commit the removal:**
   ```sh
   git commit -m "Remove p3 submodule"
   ```

3. **Delete the directory if it still exists:**
   ```sh
   rm -rf p3
   ```

4. **Now re-add the submodule:**
   ```sh
   git submodule add https://github.com/multitudes/lbrusa42-IoT-Playground.git p3
   ```

**Summary:**  
- Remove with `git rm -f p3` and commit.
- Delete the folder.
- Re-add the submodule.

## debug errors in broken submodule 
Your errors indicate that the submodule p3 is in a **broken or inconsistent state** in your local repo. This can happen if the submodule was not initialized properly, or if the index is corrupted.

### How to fix

1. **Remove the broken submodule directory:**
   ```sh
   rm -rf p3
   ```

2. **Deinitialize the submodule:**
   ```sh
   git submodule deinit -f p3
   ```

3. **Remove cached submodule data:**
   ```sh
   rm -rf .git/modules/p3
   ```

4. **Reinitialize the submodule:**
   ```sh
   git submodule update --init --recursive
   ```

If you still have issues, you can also try:

5. **Reset the index and clean up:**
   ```sh
   git rm -f p3
   git commit -m "Remove broken p3 submodule"
   git submodule add git@github.com:multitudes/lbrusa42-IoT-Playground.git p3
   git submodule update --init --recursive
   git commit -m "Re-add p3 submodule"
   ```

---

**Summary:**  
- Remove the broken submodule directory and cached data.
- Deinitialize and reinitialize the submodule.
- If needed, remove and re-add the submodule.

This should resolve the errors and restore your submodule to a working state.



# Remove
To remove a Git submodule, follow these steps:

1. Remove the submodule entry from .gitmodules:
   ```sh
   git config -f .gitmodules --remove-section submodule.p3
   ```

2. Remove the submodule entry from config:
   ```sh
   git config -f .git/config --remove-section submodule.p3
   ```

3. Remove the submodule directory and cached data:
   ```sh
   git rm --cached p3
   rm -rf p3
   ```

4. Commit the changes:
   ```sh
   git add .gitmodules
   git commit -m "Remove submodule p3"
   ```

5. (Optional) Remove the submodule's entry from .gitmodules and config manually if needed.

This will fully remove the submodule from your repository.