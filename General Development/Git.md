# Git

[[General Development]]

## Concepts
### Snapshots
Git stores data as snapshots of files and folders within a directory (git repository). Files and folders are called blobs and trees respectively

```
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```

Snapshots aren't stored as a whole, but rather as incremental changes from the previous snapshot. These changes are called commits, and each commit is linked to a parent commit, visualised as:

```
o <-- o <-- o <-- o
            ^
             \
              --- o <-- o
```

We can see three incremental commits, until we branch off into two separate streams which differ.


## Commands
`git init`: Initialises a repository in the current working directory
`git status`: Shows status of repository