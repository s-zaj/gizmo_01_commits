# Relative Refs

You can also refer to commits relative to another commit.

## the ~

```bash

# How the grandparent of the current HEAD
git show HEAD~2
```

## the ^

When looking at merge commits, we need to specify ^ to refer to the "second parent" which is the branch which you were not on when you performed the merge (that's the "first parent").

the `~` will always follow the "first parent"

## Examples

```bash
git reset HEAD~3
```

```bash
git rebase -i HEAD~2
```

### Packed-refs

Garbage collection will move the refs (branches and tags) into a single file called packed_refs.
If you are wondering why your large repository doesn't have separate files, this is to improve performance, does not affect functionality.

```bash
git gc
```

### Symbolic-refs

A "symbolic ref" is an even friendlier alias for some other ref.
In the past, switching to a different branch was done using unix symbolic links. But this was not very portable, so this concept emerged.

```bash
# Back in the day, creating a new branch 
# was just making a a symbolic link to .git/HEAD
# (don't do this)
ln -sf refs/heads/new-branch .git/HEAD
# Finding out which branch we are on would be
readlink .git/HEAD

# But this is depracated and we use git symbolic refs now
git symbolic-ref HEAD refs/heads/new-branch
```

#### HEAD

Points to the current commit that your working directory is based on. It is often a symbolic ref pointing to the branch you have checked out, like refs/heads/main.

#### FETCH_HEAD

Stores the branch or commit that was fetched from a remote repository during the last git fetch operation.

#### ORIG_HEAD

Points to the original tip of the current branch before a potentially disruptive operation like a reset, rebase, or merge. Can be useful for recovering the state.

```bash
# Undo all your hard work
git reset --hard ORIG_HEAD
```

#### MERGE_HEAD

Points to the commit(s) that you are currently merging into your branch.

#### CHERRY_PICK_HEAD

The commit that you are cherry picking.
