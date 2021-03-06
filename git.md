# Git使用教程

## Git安装配置

### Mac

Mac中可以使用homebrew来直接进行安装, 比较方便. 安装完可以直接输入`git`来查看是否安装成功

### Windows

Windows中需要先到[官网](https://git-scm.com/)下载git的安装包, 进行安装. 安装过程比较简单, 默认选项进行安装即可. 安装完成之后, 可以打开`git bash`, 并输入`git`来查看是否安装成功.

安装成功之后Mac和Windows在使用上就没有什么差别了, 基本可以互通. 唯一区别的是Mac可以直接在终端运行, 而Windows需要在安装的git bash上进行运行. 下面介绍的步骤主要是在Mac上的使用教程

### 设置全局身份

通过如下命令设置自己的名字和邮箱, 这个只是起到一个标识作用, 可以不用设置真实的邮箱.

```bash
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

使用如下命令可以取消全局设置.

```bash
git config --global --unset user.name
git config --global --unset user.email
```

## Git操作

### 创建本地仓库

创建完成之后, 目录下面会生成一个`.git`文件, 这个是存储仓库信息的, 轻易不要动.

```bash
git init
```

### 提交文件到仓库

当修改了仓库中的文件后, 我们需要通过提交操作来告诉仓库我们已经修改过了.  `-m`后面的注释是一定要填写的, 这也方便我我们之后的查询和版本回退.

```bash
git add "文件名" //将文件提交到暂存区中

git commit -m "提交的注释" //将文件提交到分支上
```

### 查看变化

```bash
git status //用于查看目前正在进行的状态, 如修改了哪些文件, 哪些文件修改后还未提交等

git diff 文件名 //用户查看指定文件修改的部分
```

### 版本回退

版本回退之前, 我们首先需要知道我们做了哪些操作, 即需要查看git的日志, 之后进行回退.

```bash
git log //这个可以看到最近的三次提交

git reset --hard HEAD^ //回退到上个版本, 如果希望回退到上上个版本, 则改成HEAD^^, 以此类推

git reset --hard HEAD~100 //回退的版本比较前的时候, 可以使用这种方法
```

而如果要把已经回退的版本恢复过来, 我们可以需要使用版本号来进行回退.

```bash
git reflog //通过查看, 来获取需要恢复的版本号
git reset --hard 版本号 //进行恢复
```

### 删除文件

删除文件, 可以直接在本地库删除之后, 然后再commit之后, 就可以彻底把库中的文件删除掉了.

没有commit之前, 可以通过如下语句把本地库进行恢复.

```
git checkout -- 文件名
```

## 远程仓库Github

### 设置SSH Key

首先运行命令行, 创建SSH Key.

```bash
ssh-keygen -t rsa -C "邮箱" //这个邮箱也不需要是真正的邮箱, 只是作为密码的
```

建立过程中会让你输入文件名和密码, 没有需要的时候就可以直接回车默认就可以了.

可以看到`~`目录新建了`.ssh`文件, 其中包含两个文件`id_rsa`和`id_rsa.pub`两个文件.  前面一个是私钥, 不能泄露. 后面一个是公钥, 可以告诉别人.

之后登录github, 点击`setting`中的`SSH Keys`, 然后进行添加. 将`id_rsa.pub`中的内容复制过去, 从而构建成SSH链接.

**这种创建方式只能创建一个SSH Keys, 每次创建新的密钥的时候会把之前的覆盖掉. 如果想要同时设立多个SSH Keys的时候, 可以看这篇[教程](https://www.cnblogs.com/Gent-Wang/p/7422433.html)**

### 通过本地库关联远程库

当自己已经建立好了一个本地库, 我们需要将其推送到远程库当中. 此时我们需要先在github上新建立一个空的库. 然后获取这个新库的链接, 并在本地输入如下语句.

```bash
git remote add origin 远程库的链接地址

git push -u origin master //把本地库的内容推送到远程库
```

第一句话是将本地的maste和远程的master关联起来. 由于远程库是空的, 所以第一次推送需要加上`-u`参数, 之后可以将命令简化. 只要本地做了修改, 就可以通过如下命令进行上传.

```bash
git push origin master
```

```在Windows中, 直接push是会失败的, 需要更新Windows的git凭证管理器才能正常使用. 直接下载```[GCMW-1.14.0.exe](https://www.jianshu.com/p/aee0813c30c5)```并运行安装即可```

### 克隆远程库直接进行使用

如果远程库中有更新的内容, 我们想克隆到本地进行查看并修改, 可以使用如下指令.

```bash
git clone 远程库的地址链接
```

之后就可以进行修改并提交到远程库中.

## 关于分支

### 建立, 合并与删除

之前的操作都是在主分支master上进行, 而在多人协作的过程中, 我们需要先在自己的分支上进行修改, 在考虑合并的主分支上, 所以我们需要掌握如何建立分支并进行合并.

```bash
git checkout -b 分支名 //创建分支并求换到创建的分支上, 这相当于两条指令, 分别进行了创建和切换

git checkout 分支名 //仅进行分支的切换

git branch 分支名 //仅进行创建分支

git branch //查看所有的分支

git merge 分支名 //将指定分支合并到当前分支上

git branch -d 分支名 //删除指定的分支
```

通常进行分支合并的时候, git一般使用的是`Fast forward`的模式, 在这种模式下, 会彻底删除分支信息. 不过通过`--no-ff`参数就可以避免这个现象.

```bash
git merge --no-ff -m "注释" 分支名 //通过这种方式进行分支合并
git branch -d 分支名 //此时删除分支依然会保留分支的信息, 可以通过日志找到版本号
```

### 解决冲突

当两个分支分别修改了源文件之后,  并提交之后. 当我们进行分支合并的时候, 我们是可以在文件中看到哪些内容是哪个分支做的, 从而选择我们需要的内容作为最终的结果, 最后进行提交, 实现合并.

### bug分支

当在一个分支开发的过程中, 主分支有一个bug需要亟待解决, 这个时候我们需要放下手头的工作去临时解决这个bug. 因此我们需要把当前还没有提交的工作隐藏起来, 现修改掉这个bug之后, 再回到现场继续工作.

```bash
git stash //将当前的工作隐藏起来

git stash list //查看保存的现场有哪些

git stash pop //恢复现场并把stash中的内容保存的现场删除
```

## 多人协作

### 不涉及分支

当多人协作的时候, 每次工作应该先获取最新的版本, 并在其上进行开发. 当要提交到远程库时, 也应该先获取最新版本, 在进行提交.

```bash
git pull //从远程服务器拉去最新版本
```

其他的详细教程可以参考这篇[文章](https://blog.csdn.net/u011535541/article/details/83379151)

