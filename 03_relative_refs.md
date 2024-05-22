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
  