# Refs

The most direct way to reference a commit is via its SHA-1 hash.

A "refs" in an indirect ways to refer to a commit. You can think of it as an alias for a commit object (hash). This is Git’s internal mechanism of representing branches and tags.

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