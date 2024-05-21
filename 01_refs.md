# Refs

The most direct way to reference a commit is via its SHA-1 hash.

A "refs" in an indirect ways to refer to a commit. You can think of it as an alias for a commit object (hash). This is Git’s internal mechanism of representing branches and tags.

Refs are stored as normal text files in the .git/refs directory

```bash
$ tree .git/refs
.git/refs
├── heads
│   ├── feature
│   └── main
└── tags
    └── v0.0.0
```




### git log

```bash
git log
```

### git show
you can view the content of a commit uif you know the commit hash

```
git show 85eca
```

### git rev-parse

* if you want to resolve a branch, tag, or another indirect reference.
* useful for parsing and normalizing various revision specifications

```bash
git rev-parse main
git rev-parse HEAD
```
