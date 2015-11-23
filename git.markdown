#Git 一些操作
1. git checkout master
# git clone 后，git软件会自动把本地的.git文件夹当成初始分支，命名为 **maser** , git checkout master就是
切换到master分支

2. git checkout -b future
# 每个人开发，都从主分支上建立一个分支，方便开发，等功能开发好了，合并到主代码去。此命令就是建立一个
future分支，并将当前的工作切换到这个分支里去，则以后的git add ,commit 影响的都是future这个分支。

3. git branch -D future 
# 删除刚才建立的future分支，这样就回到了master分支

4. git merge
# 此命令是将修改合并进来，比如修改了future分支，先执行git checkout master 回到master分支，然后进行
git merge future ，则future中修改过的地方就被合并到master分支中去了

上述流程即为 ： 建立一个分支，合并到主分支，然后删除新建的分支

##远程和本地都有更新，消除冲突
1. 先将本地的更新暂存起来
```
git stash
git pull
git stash pop
```

2. 撤销本地的更新
```
git reset --hard 
git pull origin master
```


