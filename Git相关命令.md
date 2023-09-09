---
title: Git相关命令
date: 2022-02-06 00:12:23
tags:  学习
---

##  SSH密钥文件

Github里面S设置SH公钥有两者选择方式

1. 账号下的每个仓库都设置一个公钥，因为GitHub官方要求每个仓库的公钥都不能相同，所以每个账号都要搞一个密钥（很麻烦）
2. 给账号分配一个公钥，然后这个公钥就可以在这个账号下的每个仓库中使用（推荐）

生成密钥

```bash
ssh-keygen -t rsa -C "your_email@youremail.com"
```

验证密钥是否添加成功

```bash
ssh -T git@github.com
```

## 本地仓库处理

创建仓库

```bash
git init
```

将文件添加到仓库

```bash
git add .
```

将文件提交到仓库

```bash
git commit -m "message"
```

修改最新提交的commit

```bash
git commit --amend
```

回滚到特定版本

```bash
git reset --hard HEAD^	#回滚到上一版本
git reset --hard HEAD~3	#回滚到3次版本之前，以此类推可回到n次版本之前
git reset --hard commit_id
```

查看仓库当前状态

```bash
git status
```

更改分支名称

```bash
git branch -m oldNme newName
```

## 远程仓库处理

删除远程分支（注意主分区无法删除，如果需要删除的话要在github的setting里面切换主分支）

```bash
git push --delete origin oldName
```

克隆仓库

```bash
git clone <repo>
```

连接远程仓库

```bash
git remote add origin git@github.com:yourName/repositoryname.git
or
git remote add origin https://github.com/yourName/repositoryname.git
```

从远程仓库pull到本地仓库

```bash
git pull origin main//因为黑命贵，所以master改main了。。。。这里好坑
```

从本地push到远程仓库

```bash
git push origin main
```



## .gitignore文件

当文件已经提交后才记起来没有写.gitignore文件的处理方法：

```bash
git rm -r --cache . #不要忘了后面的那个"."
```

```bash
git add .
```

 ```bash
git commit -m "重新添加.gitignore文件"
 ```

## git rebase命令

情景再现：commit太多次，想要将其合并（广义上的删除）

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220330234049303.png" alt="如图，共有4此提交" style="zoom: 67%;" />

用`git log`命令可以看到，共有4次提交，想要合并其中的“增加4.1和4.2文件”和“加入3.1和3.2文件”，将其表达为“增加3.1，3.2，4.1，4.2文件”。（可以使用`git log --pretty=oneline`命令让commit一行一行输出）

### 操作

```bash
git rebase -i commit_id(commit_id只需要简写前面六位字母即可，不需要全部填上去)
```

以上命令的`commit_id`表示的是将其前面的`commi_id`列入编辑状态，例如此处我输入`git rebase -i 88480a `，则可以对`411b01`、`5a9bb6`和`8f0dd1`这三个commit进行编辑。（也就是说分支最少会剩余2个commit）

进入编辑页面

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220331003630124.png" alt="image-20220331003630124" style="zoom:67%;" />

前缀的意思：

1. `pick`保留该commit，缩写：`p`
2. `squash`使用该commit但是**合并**到前一个**老**的commit之中去，缩写：`s`，在某种意义上来说就是删除。该步骤结束后会弹出窗口对合并后的commit进行编辑
3. `reword`类似于`pick`，但是会弹出窗口，可以修改commit的信息，缩写：`r`
4. `edit`类似于`reword`可以修改commit，会将commit的修改放置在接下来的amending中，也比较方便，缩写：`e`
5. `fixup`和`squash`类似，但是会直接舍弃其commit信息，缩写：`f`
6. `exec`执行shell命令（没用过）
7. `drop`删除某一commit

此处将“增加4.1和4.2文件”前的`pick`更改为`squash`（`fixup`也可以，就不会弹出窗口了），关闭编辑器，会弹出以下窗口

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220331111257663.png" alt="image-20220331111257663" style="zoom:67%;" />

默认最后的commit会有两个合并的commit共同组成，也可以在这里进行修改，例如我改成这样一行显示的：

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220331111455061.png" alt="image-20220331111455061" style="zoom:67%;" />

合并结果就会变成

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220331111542047.png" alt="image-20220331111542047" style="zoom:80%;" />

**PS**：每次操作之后记得都要`ctrl+s`保存

### 关于noop

当我们选取了最新的commit之后，就会显示noop，表示在这之前没有更新的commit可供操作，例如在本例中我输入`git rebase -i fb083c`，则会显示noop：

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220331111830269.png" alt="image-20220331111830269" style="zoom:80%;" />

## 有关报错的处理方式

### unable to auto-detect email address

![en-us_image_0000001645684408](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/en-us_image_0000001645684408.png)

原因是未设置email地址，解决方式其实上面已经给出了：

```bash
git config --global user.name ""
git config --global user.email ""
```

