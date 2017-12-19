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

### 1. show local branch
```bash
  git branch
```

### 2. show remote branch
```bash
  git branch -r
```

### 3. create a new branch
```bash
  git checkout -b <branchname>
```

### 4. switch branch
```bash
  git checkout <branchname>
```

### 5. delete branch
```bash
  git branch  (-d | -D) <branchname>
```

### 6. delete remote branch
```bash
  git push --delete origin <branchname>

  or

  git branch -d -r <branchname>
  git push origin :<branchname>
```

### 7. rename branch name
```bash
  git branch (-m | -M) <oldbranch> <newbranch>
```

# tag

### 1. list tags
```bash
  git tag
```

### 2. delete local tag
```bash
  git tag -d [tag]
```

### 3. delete remote tag
```bash
  git push origin :refs/tags/[tagName]
```

### 4. show tag message
```bash
  git show [tag]
```

### 5. push specific tag
```bash
  git push [remote] [tag]
```

### 6. push all of tags
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
