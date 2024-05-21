# Refs

The most direct way to reference a commit is via its SHA-1 hash.

"refs" are indirect ways to refer to a commit

## tools


### git log

```bash
git log
```

### git show
you can view the content of a commit uif you know the commit hash

```
git show 4138
```

### git rev-parse

* if you want to resolve a branch, tag, or another indirect reference.
* useful for parsing and normalizing various revision specifications

```
git rev-parse main
git rev-parse HEAD
```
