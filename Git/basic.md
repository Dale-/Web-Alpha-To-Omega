# create a new repository
 ```bash
  git init

  git init [project-name]

  git clone [url]
 ```

# add & commit
```bash
   git add <filename>
   git commit -m "Commit message"
```

# pushing changes
```bash
  git push origin <branchname>
```

# branching

### show local branch
```bash
  git branch
```

### show remote branch
```bash
  git branch -r
```

### create a new branch
```bash
  git checkout -b <branchname>
```

### switch branch
```bash
  git checkout <branchname>
```

### delete branch
```bash
  git branch  (-d | -D) <branchname>
```

### delete remote branch
```bash
  git push --delete origin <branchname>

  or

  git branch -d -r <branchname>
  git push origin :<branchname>
```

### rename branch name
```bash
  git branch (-m | -M) <oldbranch> <newbranch>
```

# tag

### list tags
```bash
  git tag
```

### delete local tag
```bash
  git tag -d [tag]
```

### delete remote tag
```bash
  git push origin :refs/tags/[tagName]
```

### show tag message
```bash
  git show [tag]
```

### push specific tag
```bash
  git push [remote] [tag]
```

### push all of tags
```bash
  git push [remote] --tags
```

# config
```bash
  git config --list

  git config -e [--global]

  git config [--global] user.name "[name]"
  git config [--global] user.email "[email address]"
```
