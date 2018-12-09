## Squash Commit

### 模拟三次Commit记录

```javascript
    mkdir GitPractise
    cd GitPractise

    git init

    echo "0" >> a
    git add a
    git commit -m "Commit-0"

    echo "1" >> a
    git add a
    git commit -m "Commit-1"

    echo "2" >> a
    git add a
    git commit -m "Commit-2"

    echo "3" >> a
    git add a
    git commit -m "Commit-3"
```


```javascript
    * ddca165 - (HEAD -> master) Commit-3
    * c1f11cb - Commit-2
    * e7432d3 - Commit-1
    * 723e4c4 - Commit-0
```

### 那么我们如何合并commit-1 ~ commit-3呢

问题：如何把`e7432d3 - Commit-1`、`c1f11cb - Commit-2`、`ddca165 - Commit-3`合并到一起只保留`e7432d3 - Commit-1`的Git message `Commit-1`呢

> git rebase -i

`-i` 实际上就是 `--interactive` 的简写。

```javascript
    git rebase -i 723e4c4
```

当然，我们也可以通过 `HEAD~3` 来制定该 Commit:
```javascript
    git rebase -i HEAD~3
```

按下回车后，我们会进入到这样一个界面：

```javascript
    pick e7432d3 Commit-1
    pick c1f11cb Commit-2
    pick ddca165 Commit-3

    # Rebase 723e4c4..ddca165 onto 723e4c4 (3 command(s))
    #
    # Commands:
    # p, pick = use commit
    # r, reword = use commit, but edit the commit message
    # e, edit = use commit, but stop for amending
    # s, squash = use commit, but meld into previous commit
    # f, fixup = like "squash", but discard this commit's log message
    # x, exec = run command (the rest of the line) using shell
    #
    # These lines can be re-ordered; they are executed from top to bottom.
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    #
    # However, if you remove everything, the rebase will be aborted.
    #
    # Note that empty commits are commented out
```

上面未被注释的部分列出的是我们本次rebase操作包含的所有提交，下面注释部分是git为我们提供的命令说明。每一个commit id前面的pick表示指令类型，git为我们提供了一下几个命令：

> pick: 保留该commit(缩写:p)
> reword: 保留该commit，但我需要修改该commit的注释(缩写:r)
> edit: 保留该commit，但我要停下来修改该提交（不仅仅修改注释）(缩写:e)
> squash: 将该commit和前一个commit合并(缩写:s)
> fixup: 将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
> exec: 执行shell命令(缩写:x)
> drop: 我要丢弃该commit(缩写:d)

所以我们可以使用squash命令完成我们需要的功能

```javascript
    pick e7432d3 Commit-1
    squash c1f11cb Commit-2
    squash ddca165 Commit-3
```

当然我们可以使用它的缩写：
```javascript
    pick e7432d3 Commit-1
    s c1f11cb Commit-2
    s ddca165 Commit-3
```

我们可以使用`:wq`保存并推出。这个时候，我们进入到下一个界面

```javascript
    # This is a combination of 3 commits.
    # The first commit's message is:
    Commit-1

    # This is the 2nd commit message:

    Commit-2

    # This is the 3rd commit message:

    Commit-3

    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    #
    # Date:      Tue Jan 5 23:27:22 2018 +0800
    #
    # rebase in progress; onto 5d39ff2
    # You are currently editing a commit while rebasing branch 'master' on '723e4c4'.
    #
    # Changes to be committed:
    #   modified:   a
```

我们保留我们想要的commit message，或者自定义一个commit message

```javascript

    Commit-1

    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    #
    # Date:      Tue Jan 5 23:27:22 2018 +0800
    #
    # rebase in progress; onto 5d39ff2
    # You are currently editing a commit while rebasing branch 'master' on '723e4c4'.
    #
    # Changes to be committed:
    #   modified:   a
```

完成commit message后，我们可以再看一下我们的log

```javascript
    * 2d7b687 - (HEAD -> master) Commit-1
    * 723e4c4 - Commit-0
```