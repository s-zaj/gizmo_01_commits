# Refs

The most direct way to reference a commit is via its SHA-1 hash.

A "ref" is an indirect way to refer to a commit. You can think of it as an alias for a commit object (hash). This is Git’s internal mechanism of representing branches and tags.

Refs are stored as normal text files in the .git/refs directory

```bash
$ tree .git/refs
.git/refs
├── heads
│   ├── local_wip
│   └── main
├── remotes
│   └── origin
│       └── main
├── stash
└── tags
    └── v0.0.0
```

```bash
# view the log of commits

git log

# Inspect the commit hash at the tip of the `main` branch:
git log -1 main

# This should correspond to the contents of the file .git/refs/heads/main
cat .git/refs/heads/main

# git show
# you can view the content of a commit if you know the commit hash 

git show 85eca
git show $(git log HEAD^)
```

### packed-refs

Garbage collection will move the refs (branches and tags) into a single file called packed_refs.
If you are wondering why your large repository doesn't have separate files, this is to improve performance, does not affect functionality.

```bash
git gc
```


### git symbolic-ref

A "symbolic ref" is a friendlier alias for some other ref.
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

Points to the original tip of the current branch before a potentially disruptive operation like a reset, rebase, or merge.

Can be useful for recovering the state before such an operation was performed

```bash
# Undo all work and go back to the state you were before you did the thing
git reset --hard ORIG_HEAD
```

#### MERGE_HEAD

Points to the commit(s) that you are currently merging into your branch.

#### CHERRY_PICK_HEAD

The commit that you are cherry picking.

### git rev-parse

* if you want to resolve a branch, tag, or another indirect reference.
* useful for parsing and normalizing various revision specifications

```bash
git rev-parse main
git rev-parse HEAD

# Get the full commit hash from a part of it
git rev-parse 85ec

# Do we have a tag?
TAG="refs/tags/v0.0.0"
git rev-parse --verify $TAG
```

Some other uses for scripting:

```bash
# Get the name of the current branch
git rev-parse --abbrev-ref HEAD

# Get the absolute path of the repository
git rev-parse --show-toplevel
```

### Exercises

`Can we move the tag "v0.0.0" to another commit without ever creating a branch?`

Git likes to have branches, and it's difficult to track commits that are not on any branch.

But it's possible to make a commit without a branch. This is called the "Detached HEAD" state (you've probably ended up there before)

#### 1) Checkout a hash instead of a ref

```bash
# Checkout the latest commit on main, but detaching from main
$ git checkout $(git rev-parse main)

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

# Asking which branch we are on tells us which commit we detached from the tree
$ git branch

* (HEAD detached at e59b306)
```

#### 2) Make a commit and tag

```bash
echo "The Lost Ark" > 13_the_lost_ark.md
git add -A && git commit -m "my best multi-lingual pun so far"

git tag v0
```

If we switch to the main branch, Git will warn us. Let's disect the warning

```bash
$ git checkout v0

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

# "you may do so (now or later)"

# We get warned again when switching to main

$ git checkout main

Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  c8446db my best multi-lingual pun so far

If you want to keep it by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> c8446db

# The tag still exists
git rev-parse --verify v0
git checkout v0
```
