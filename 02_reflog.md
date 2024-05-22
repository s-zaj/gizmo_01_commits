# Reflog: Git'sSafety Net

It records chronologically almost every change you make in your repository, regardless of whether you committed or not.

```bash
git reflog
```

We reference commits stored in the reflog using the `HEAD{}` syntax.


```bash
git checkout HEAD@{32}
```

### Exercises

Previously we created a tag on a commit which we made while in the Detached HEAD state.
Life continues, and we switch to the main branch.

Then we delete the tag by mistake.The commit is still in the database, because we only removed the tag. But how would we get back to it?

```bash
# We could write down the commit hash.

git checkout main
git rev-parse --verify v0
git checkout v0
```

```bash
# Simulate fat fingers by deleting the tag

$ git tag -d v0
Deleted tag 'v0' (was 37b8d2a)
```

```bash
# Try to checkout the ref

$ git checkout v0
error: pathspec 'v0' did not match any file(s) known to git

# Commit still exists but we have no nice ref (alias) to get back to it

$ git show 37b8d2a

commit 37b8d2a07c2020ed6d3c09bc9ec52b12bdbd3d15 (tag: v0.0.0)
Author: s-zaj <aelialper@gmail.com>
Date:   Wed May 22 09:59:52 2024 +0200

    my best multi-lingual pun so far

added: 13_the_lost_ark.md
```

```bash
# What if we are in a context where we don't have the hash in the terminal output anymore?

# Use reflog to see the chronology of the actions you performed
git reflog
```

```bash
# Note on using 'git tag' instead of just writing to regular text file refs/tags/v0

git checkout $(git rev-parse main)
echo "The Lost Ark" > 13_the_lost_ark.md
git add -A && git commit -m "my best multi-lingual pun so far"

# We could do this and it would work
echo $(git rev-parse HEAD) > .git/refs/tags/v0

# But it does not get recorded in the reflog in the same way
git reflog

```

