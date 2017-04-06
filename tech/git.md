# 使用git中的一些注意事项

在团队协作中，git的使用时很常用的。今天在工作过程中，遇到以前使用git时候没有注意的一些方面，籍着本文，顺便记下来，以便日后遇到相同情况下可以查看。

初始化仓库命令：
```
git init
```

查看仓库状态：
```
git status
```

加入到缓存区：

```
git add 
```

从缓存区删除文件 （当文件已经上传到github时，又不想上上传了，可以先在本地缓存区删除，然后配置好.gitignore文件，然后再次上传)

```
git rm --cached [filename]
```

确认修改：

```
git commit -a -m
```
 
克隆一个仓库：
```
 git clone [resposity adress]
```

查看远程仓库：

```
git remote -v
```

增加远程仓库：

```
git  remote add [resposity name] [resposity adress]
```

移除远程仓库：

```
git remote delete [resposity name]
```

从远程仓库拉取更新：

```
git fetch [remote resposity name]
```

将远程分支合并到本地分支
```
git merge [remote branch name]
```

直接将远程跟新拉去到本地分支

```
git pull [remote resposity name]
```

推送到远程分支

```
git push [remote resposity name] [local branch name]:[remote branch name]
```







留白一下，以便日后有什么增加。














一般来说克隆一个仓库，我们会直接用git clone 命令，然后直接在仓库里面进行操作。

git clone 命令相当于 git add [仓库名字] , git fetch [仓库名字]，git merge [远程分支] ,一连串得做了三个操作。

需要记住，拉去fetch remote resposity，只是更新了远程分支，并没有把远程分支合并到本地，需要我们手动去合并。

有时候，我们已经在master分支进行了操作，又想从远程分支拉去文件，并一并更新到远程分支.

比如：我们在项目里面新建了a.js，然后remote add 远程仓库，然后git fetch 远程放仓库,然后git merge 远程分支，想要将a.js加入到远程仓库中。

但是git是不允许的，因为merge命令，之允许有相同base的branch进行合并。

git依然提供了命令让我们强行合并

```
git merge --allow-unrelated-histories [remote branch name]
```

这样，我们就可以将两个base不同的分支合并。




还有一点就是，我们要将文件强行推送到远程仓库里面。文件推送到远程仓库前，远程仓库会检查远程仓库是否推送的分支的ancestor，如果不是，则reject推送。

强行推送也是可以的，命令如下：

```
git push -f [remote resposity name]
```

-f命令强制推送，但是这样做有一个后果就是，会删除之前所有的commits，所以这个方法请谨慎使用。


