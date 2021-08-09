# **Git:**

## 一：提交拉取

| 命令           | 说明                                 | 注释                                               |
| -------------- | ------------------------------------ | -------------------------------------------------- |
| **git clone**  | 克隆一个仓库到当前目录               |                                                    |
| **git add**    | (在有新文件时使用),将文件添加到索引  | 只有被添加到索引的文件才能提交及检查到是否存在变更 |
| **git commit** | 将文件提交到本地仓库                 | **提交的时候一定要添加注释: *-m “XXX”***           |
| **git push**   | 将文件推送到远程查看提交日志仓库     |                                                    |
| **git pull**   | 更新代码或者拉去代码                 |                                                    |
| **git fetch**  | 从另外一个仓库下载一个对下那个和引用 |                                                    |



## 二：分支和冲突解决

| 命令                      | 说明                                                         | 注释 |
| ------------------------- | ------------------------------------------------------------ | ---- |
| **git checkout XXX**      | 切换到某个分支                                               |      |
| **git branch**            | 查看当前所在分支                                             |      |
| **git branch -a**         | 查看所有分支                                                 |      |
| **git branch XXX**        | 创建XXX分支                                                  |      |
| **git merge**             | 合并分支                                                     |      |
| **git diff**              | 显示两个提交，或提交与工作区之间的差异                       |      |
| **git rebase**            | 逐个文件进行合并，如果出现冲突需要手动解决，可以在合并过程之中取消合并，并且回滚代码 |      |
| **git rebase --continue** | 继续合并                                                     |      |
| **git rebase --skip**     | 跳过                                                         |      |
| **git rebase --abort**    | 取消合并，回滚代码                                           |      |



## 三：仓库

| 命令         | 说明                             | 注释 |
| ------------ | -------------------------------- | ---- |
| **git init** | 初始化本地仓库(创建一个本地仓库) |      |
|              |                                  |      |



## 四：日志及状态

| 命令           | 说明                   | 注释 |
| -------------- | ---------------------- | ---- |
| **git log**    |                        |      |
| **git show**   | 显示仓库分支列表(待定) |      |
| **git status** | 显示工作区状态         |      |



## 五：创建项目并与远端仓库关联

```
1.git init
2.git add .
3.git commit -m "这里添加注释"
4.git remote add origin https:github.com/weibangkong/xxxx.git
5.git push -u origin master
```



## 六：分支合并

```
e.g.     将uat 分支合并到master上
1.git checkout uat
2.git pull
3.git checkout master
4.git merge uat
5.git pull
```

## 七: 分支比较

### 1.查看A分支有而B分支没有的提交内容

```java
git log BRANCH_A ^BRANCH_B
```

### 2.查看A分支比B分支多的内容

```java
git log BRANCHE_B..BRANCH_A
```

### 3.对比两个分支又什么不同

```java
git log BRANCH_A...BRANCHE_B
```

### 4.两个分支有何不同并显示提交内容所属分支

```java
git log --left-right BRANCH_A...BRANCHE_B
```