# Git: Branches, Merges, and Remotes

## Tree-ish

### Git references

* SHA1 hash: the commit ID
* HEAD
* Branch
* Tag
* Ancestry
    * Parents: abcd1234^, master^, HEAD^, HEAD~1, HEAD~
    * Grandparent: abcd1234^^, master^^, HEAD^^, HEAD~2
    * Grat-Grandprarents: abcd1234^^^, master^^^, HEAD^^^, HEAD~3

        ```git
        git show HEAD^
        git show HEAD^^
        git show HEAD~3
        ```

### Tree Listing
List of blobs (binary large object = file) and trees (tree =  directory) of a tree

```git
git ls-tree HEAD
git ls-tree HEAD^
git ls-tree HEAD tree/
```

Filter the commit log after a commitID to HEAD

```git
git log abcd1234..
git log abcd1234..HEAD
git log <SHA>..<SHA>
```

Filter the commit log by file or directory

```git
git log filename
git log dirname
```

## Branches
Create a new branch:
```git
git branch branch_name
```

!!! note "Note that new branch and its parent branch are the same at the moment. There are not differences because there were not commits at the moment."

List all branches. Currently checkout branch is shown in green color: 
```git
git branch
```

Switch branches
```git
git checkout branch_name
```

Switch to a new branch
```git
git checkout -b branch_name
```

Switching options with uncommited changes:

1. commit the changes to the current branch
2. checkout the file again and remove the changes
3. stash the changes

Compare branches 
```git
git diff master..new_feature
git diff --color-words <SHA>..<SHA>
```
!!! info "As a general rule, the older branch should go firsts, so changes shown have occurred since that point in time."

Find out which other branches already **have merged** all their commits into this branch
```git
git branch --merged
```

Find out which other branches **have NOT merged** their commits into this branch yet
```git
git branch --no-merged
```

Rename branches
```git
git branch -m new_branch_name
```

Delete branches
```git
git branch -d branch_name
```

!!!warning "Cannot delete the checked out branch"  
!!!warning "Cannot delete branches not fully merged. Instead, use capital `-D`, but commits in the branch will be lost."

### Reset Branches

!!! tip "Back up the latest commitID before, in case you want to undo the reset using `git reset <mode> commitID`."

* **Soft** reset: Latest commits changes are staged and pending to commit. useful when you want to back things up in the commit timeline.
    ```git
    git reset --soft <SHA>
    ```

* **Mixed** reset (default reset option): Latest commits changes are unstaged, pending to stage and commit.
    ```git
    git reset --mixed <SHA>
    git reset <SHA>
    ```

* **Hard** reset: Latest commits changes are nowhere. Useful to permanently undo commits (SHA=commitID) or if you want to make one branch look like another (sha=branchID)
    ```git
    git reset --hard <SHA>
    ```

    !!! tip "Create a new branch as backup before a hard reset"

### Merge Branches