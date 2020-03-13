# 创建仓库

## 初始化创建

- ```bash
  git init
  ```

  - cd到某个文件夹下, 使用该命令, 即可完成git仓库的初始化

## clone创建

- ```bash
  git clone https://github.com/marcel0637/gitskills.git
  ```

  - 需cd到某个文件夹, 然后使用命令

# 添加文件

- ```bash
  git add <file>
  ```

  - 可反复使用,然后一次commit

- ```bash
  git commit -m <message>
  ```

# 状态获取

- ```bash
  git status
  ```

- ```bash
  git diff
  ```

  - 在git status中查询到有文件被修改后, 可使用如 git diff xxx.txt 来比较获取修改内容

# 版本修改

## 查看提交历史

- ```bash
  git log
  ```

## 查看命令历史

- ```bash
  git reflog
  ```

注意 : 两种历史查看均可获得ID, 从而进行接下来的版本修改

## 版本修改

- ```bash
  git reset --hard HEAD^
  ```

  - HEAD代表当前版本, 一个^表示回退一个版本, 也可写成HEAD~100表示回退100个版本

- ```bash
  git reset --hard 版本ID
  ```

注意 : git的版本修改只是移动HEAD指针, 所以速度非常快

# 工作区和暂存区

## 工作区

工作区就指我们的文件夹

## 版本库

- ### 暂存区

- ### 分支master(仓库)

git add 把文件从工作区 $\Longrightarrow​$  暂存区

git commit 把文件从暂存区 $\Longrightarrow$  仓库

# 管理修改

## 撤销修改

- ```bash
  git checkout -- file
  ```

  - 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时

- ```bash
  git reset HEAD <file>
  ```

  - 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改, 则先使用该命令回到上一个场景, 再使用第一条命令回复到初始状况

  注意 : 如果提交到了版本库的话, 想要撤销提交需进行版本修改 ( 前提是没有推送到远程仓库 )

## 文件删除

- ```bash
  git rm test.txt
  git commit -m "remove test.txt"
  ```

  适用于工作区删除文件后, 想把本次删除提交上去

- ```bash
  git checkout -- test.txt
  ```

  使用于工作区误删除

# 远程仓库

## 获得远程github仓库的推送权限

在github中添加本地电脑的SSH KEY即可

## 项目关联远程仓库

```bash
git remote add origin https://github.com/marcel0637/gitskills.git
```

## 推送内容

- 第一次推送

  ```bash
  git push -u origin master
  ```

- 后续推送

  ```bash
  git push origin master
  ```

# 分支管理

## 创建与合并分支

- 创建分支

  ```bash
  git branch <name>
  ```

- 切换分支

  ```bash
  git checkout <name>    
  ```

- 创建+切换分支

  ```bash
  git checkout -b <name>
  ```

- 查看分支

  ```bash
  git branch
  ```

  **结果标*代表为当前分支**

- 合并某分支到当前分支

  ```bash
  git merge <name>
  ```

- 删除分支

  ```bash
  git branch -d <name> //被合并过后删除该分支时使用
  git branch -D <name> //未被合并过的时候删除该分支时使用
  ```

**注意** : 由于 git 的分支操作是基于指针的操作, 所以速度十分快

## 合并冲突解决

当合并的两个分支处于平级且存在不同时, 使用合并会出现冲突, 使用git status可用查看出现冲突的文件

出现冲突的文件会被修改, 我们可直接打开进行编辑, 然后重新add和commit, 即可完成手动解决冲突并合并

可使用下面的命令查看分支合并图

```bash
git log --graph --pretty=oneline --abbrev-commit
```

## 分支管理策略

合并分支时, 可以禁用fast forward模式, 这样的话在合并分支的时候, 会保留原始分支的信息, 而不会进行删除

```bash
git merge --no-ff -m "merge with no-ff" dev
```

**注意** : fast forward模式下, 合并看不出来曾经做过合并, 但禁用后可以

**实际使用** : master分支仅用来发布新版本, 干活在dev分支上, 我们只需把做好的分支往dev上合并即可. 

## Bug分支

场景 : 我们正在A分支中进行工作, 此时需要到B分支中处理Bug, 则我们需要保存A分支中的工作现场

```bash
git add * //这一步并不是必须的, 但必须保证stash前 没有 从未add过的文件
git stash //保存工作现场
```

然后我们可以切换到B分支, 开启新的处理Bug分支, 处理完后再把处理Bug分支和B分支合并, 并删除处理Bug分支

最后我们切换回A分支, 还原之前的工作现场

```bash
git stash apply stash@{0} //恢复方式1,不进行存储现场的清除
git stash drop //恢复方式1对于清除方式
git stash pop //恢复方式2,从stash中取出并删除
```

假如A分支也存在B分支中的Bug, 我们可以使用如下命令进行特定提交复制

```bash
git cherry-pick <commitid> //注意命令必须在A分支中进行
```

## 多人协作

- 查看远程库信息

  ```bash
  git remote -v
  ```

  **注意** : 本地新建的分支如果不推送到远程，对其他人就是不可见的；

- 在本地创建和远程分支对应的分支

  ```bash
  git checkout -b branch-name origin/branch-name
  ```

  **注意** : 使用本地和远程分支的名称最好一致

- 建立本地分支和远程分支的关联

  ```bash
  git branch --set-upstream branch-name origin/branch-name
  ```

- 从远程抓取分支，使用如下命令. 如果有冲突，要先处理冲突

  ```bash
  git pull
  ```

- 从本地推送分支，使用如果推送失败，先使用上一个命令抓取远程的新提交

  ```bash
  git push origin branch-name
  ```

  **注意** : 一般只推送master和dev

# 标签管理

## 创建标签

- ```bash
  git tag <tagname>
  git tag <tagname> <commit id>
  ```

  命令用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

- ```bash
  git tag -a <tagname> -m "blablabla..."
  ```

  命令作用同上, 不过添加指定参数可以指定标签附带信息；

- ```bash
  git tag //查看所有标签
  git show <tagname> //查看指定标签详细信息
  ```

## 操作标签

- 推送一个本地标签到远程仓库

  ```bash
  git push origin <tagname>
  ```

- 推送全部未推送过的本地标签到远程

  ```bash
  git push origin --tags
  ```

- 删除一个本地标签

  ```bash
  git tag -d <tagname>
  ```

- 删除一个远程标签

  ```bash
  git push origin :refs/tags/<tagname>
  ```