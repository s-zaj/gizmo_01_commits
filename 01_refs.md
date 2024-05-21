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

The HEAD ref points to the current commit that your working directory is based on. It is often a symbolic ref pointing to the branch you have checked out, like refs/heads/main.

#### FETCH_HEAD

FETCH_HEAD is a ref that stores the branch or commit that was fetched from a remote repository during the last git fetch operation.

#### ORIG_HEAD

ORIG_HEAD is a ref that points to the original tip of the current branch before a potentially disruptive operation like a reset, rebase, or merge.

Can be useful for recovering the state before such an operation was performed

```bash
# Undo all work and go back to the state you were before you did the thing
git reset --hard ORIG_HEAD
```

#### MERGE_HEAD

MERGE_HEAD is a ref that points to the commit(s) that you are currently merging into your branch.

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

## exercises

Can we move the tag "v0.0.0" to another commit without ever creating a branch?

In git, we generally work with branches, that's the recommended way. But it's possible to make a commit without a branch. This is called the "Detached HEAD" state. You have probably ended up there unintentionally.


.

.

.

.

.

.

.

.

.

.

.

.

.

.

1) Checkout a hash instead of a ref
2) Make a commit
3) Modify the ref for the tag


```bash
git checkout $(git rev-parse main)
git branch

echo "The Lost Ark" > 13_the_lost_ark.md
git add 13_the_lost_ark.md
git commit -m "my best multi-lingual pun this year"

echo $(git rev-parse HEAD) > .git/refs/tags/v0.0.0
git checkout v0.0.0
```