## **Git 总结**

### Git 命令介绍：

 #### 下载 git 安装，git --version 查看是否安装成功
 #### git 配置用户名邮箱：

    - git config --global|--local|--system [user.name](http://user.name/) ymj
    - git config --global|--local|--system user.email ymj2959@126.com
    - git config --list 查看配置信息

 #### 创建新仓库：

    - git init： 项目代码已存在 进入项目文件夹，然后执行 git init（或者手动创建文件夹，进入后执行 git init）
    - git init projectName： 项目代码不存在，执行会创建这个文件夹并生成版本库

 #### 检出仓库：

    - git clone username@host:/path/to/repository： 克隆远端仓库到本地这里
    - git clone /path/to/repository： 克隆本地仓库/path/to/repository 到这里

 #### 创建、切换|检出分支：

    - git checkout -b feature_x: 基于当前的分支创建并切换到 feature_x 特性分支
    - git branch \[-v|-av\] ：查看分支
    - git branch branchName ：创建 branchName 分支
    - git checkout branchName：切换到 branchName 分支
    - git switch branchName：切换到 branchName 分支

  #### 添加和提交：

    - git add file1 file2：添加到暂存区（把工作区改动的文件，包括新增的）
    - git add \*：简写，一次添加所有改动
    - git add -u：所有已经被跟踪后有修改的文件一起添加到暂存区
    - git commit -m "代码提交信息"：把暂存区的文件提交到历史版本库（提交到了 HEAD，但是还没到远端仓库）
    - git commit --allow-empty -m "xxx"：创建一个空的 commit
    - git commit -am"xxx":

  #### 文件重命名：
    - git mv index.html home.html
    - 方法一：
      - 将一个文件重命名后，git status 会发现 git 的记录是：删掉了原文件同时新增了个文件
      - 那么此时的做法是：git add 新文件名 将新文件添加到暂存区里去，同时用 git rm 原文件名 来删除旧的文件；
      - 然后在 git status 会显示，git 知道你是将原文件更名（renamed）为了新文件；
        - ![](https://api2.mubu.com/v3/document_image/9a291618-ca94-4611-856f-ce3cc1f4376c-11568850.jpg)

    - 方法二：git mv index.html home.html

      - 推荐重命名用这种方式
        - ![](https://api2.mubu.com/v3/document_image/ac405c08-bede-4239-a066-4a7af7a26b3d-11568850.jpg)

  #### 删除不要的文件：

    - git rm file 即可，不需要先去从工作区手动删除

  #### 删除不要的分支：git branch -d 分支名

    - git branch -d 分支名：删除已经合并过后不需要了的分支；
    - 如果这个分支还没 merge，不允许删除的话，可以使用 git branch \-D 分支名强制删除，但需要考虑清楚是否真的要删除；

      - - ![](https://api2.mubu.com/v3/document_image/b1725a11-243c-46a2-88f6-9bf2c72b20ae-11568850.jpg)

  #### 修改最新 commit 的 message：git commit --amend

    - 此时会进入编辑状态，编辑完 message 信息后按 esc 退出编辑状态（仍然是编辑界面），:wq 退出编辑页面并保存修改，会发现修改已成功，可以打日志看一下结果

  #### 修改老旧 commit 的 message：git rebase -i commit 需要修改的 commit 的父亲的 id（哈希值）

    - 改 pick 成 r（reword）：git reword --amend 然后:wq! 然后修改 message 再:wq! 即完成
    - 注意这个首先是在本地分支的没有提交到远程分支的
    - 其次这个中间会有一个分离头指针 等改完后会在回到当前分支（人、git rebase 的时候） 这里是分两步的；而且当前分支的其他信息不变但是 id 变了（最新一次 commit 的 id）

  #### 恢复文件状态：

    - 让暂存区恢复成 HEAD 状态：git reset HEAD
    - 让暂存区部分文件恢复成 HEAD 状态：git reset HEAD \-- index.html
    - 让工作区恢复成暂存区的状态：git checkout \-- index.html

  #### 消除最近的几次提交：

    - 切换到要消除的那个分支，然后：git reset --hard 要丢掉的几次提交的父提交的 id

  #### 推送改动：

    - git push origin branchName：将当前分支的内容提交到远端仓库的 branchName 分支；
    - git remote add origin username@host:/path/to/repository：还没有克隆远端仓库的话，需要此命令添加跟远端仓库的关联，origin 是给远端仓库起的别名，可以用其他的，但是以后得记住这个名字，一般都是用的 origin，可以用 git remote -v 来查看，用 git remote rename oldName newName 来修改；然后再执行上一步的 git push 命令；
    - git remote -v： 查看是否与远端仓库有过关联

  #### 更新与合并：

    - git pull：更新本地仓库至最新改动（在你的工作目录中 获取（fetch） 并 合并（merge） 远端的改动）。
    - git fetch：在你的工作目录中 获取（fetch）但不合并（merge） 远端的改动，需要再做 merge 操作；
    - git merge branchName：合并其他分支 branchName 到你的当前分支；
    - git merge \--allow-unrelated-histories branchName：允许将不相关的分支合并到当前分支；
    - 注意：在上述 pull 和 merge 两种情况下，git 都会尝试去自动合并改动。遗憾的是，这可能并非每次都成功，并可能出现冲突（conflicts）。
    - 这时候就需要你修改这些文件来手动合并这些冲突（conflicts）。改完之后，你需要执行如下命令以将它们标记为合并成功：

      - git fetch origin
      - git add file
      - git commit ...
      - git merge branchName
      - 上述四步可以用 git pull 代替
      - git push origin branchName

    - 如果冲突后不想合并了：git merge --abort：取消合并

  #### 替换本地和暂存区的改动：

    - git checkout -- <filename>：使用 HEAD 中的最新内容替换掉你的工作目录中的文件，操作失误时用的上；
    - git fetch origin 然后 git reset --hard origin/master：丢弃本地的所有改动、暂存与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它；
    - git reset --hard：将暂存区和工作目录上的变更都清理掉（相当于用上一次的提交来覆盖暂存区和本地工作目录上的修改），这个操作相对来说比较危险，因为容易把你更改的东西弄丢。而且，这个不会破坏 commit 的提交记录。

  #### stash 暂存与恢复：有临时紧急任务时，将当前改动先暂存到 git 堆栈，

    - 有临时紧急任务时，将当前工作区改动先暂存到 git 堆栈，回头再恢复到工作区；
    - git stash：本地工作区备份

      - 将当前工作区修改暂存进 git 堆栈里，然后工作区内容恢复到仓库 HEAD 的内容（最后一次提交的内容）

    - git stash list：查看本地工作区的备份列表
    - git stash pop: 恢复备份到本地工作区

      - 弹出 git 堆栈中的最近一条备份并与本地工作区自动合并；如果成功则此备份自动从 git 堆栈中删除；如果有冲突需要手动解决，然后用 git stash drop 手动删除这条内容；

    - git stash apply：恢复备份到本地工作区但不删除备份

      - 应该是需要用 git stash drop 手动删除

    - git stash drop: 手动删除工作区备份
    - git stash apply stash@{0}：恢复指定备份到工作区

      - 应该有对应的 git stash drop stash@{0}

  #### 打标签：为软件发布创建标签

    - git tag 1.0.0 1b2e1d63ff ：基于这个 commit ID 创建一个名为 1.0.0 的标签；

  #### 预览提交差异：

    - git diff <source_branch> <target_branch>：预览差异
    - git diff HEAD commitId： 预览上一次提交（HEAD）跟某一个固定提交的差异
    - git diff --cached：比较暂存区和 HEAD 的差异
    - git diff ：比较工作区和暂存区的差异

  #### 查看日志：

    - 注意与 git status 区分开，这个是查看本地暂存区情况；
    - git log：了解本地仓库的历史记录；
    - git log --author=bob：只看某一个人的提交记录；
    - git log --pretty=oneline：一条提交记录只占一行的输出；
    - git log --oneline：同上，一条提交记录只占一行的输出
    - git log --all：查看本地和远端所有提交记录；
    - git log -n4：查看本地最后四次提交记录；
    - git log --graph --oneline --all：树形结构来展示所有的分支, 每个分支都标示了他的提交 ID 和提交 message
    - git log --graph --oneline --decorate --all：树形结构来展示所有的分支, 每个分支都标示了他的提交 ID、名称、标签和提交 message
    - git log --help：更多的 log 参数参考

  #### 内建的图形化 git：gitk --all，图形界面看版本演进历史

    - gitk：会弹出一个图形界面工具，注意这个图形化界面打开的时候，原来的命令行的地方敲命令是不会生效的，需要图形界面退出或关闭后才能生效；可以在这里打标签；

  #### git 实用小贴士：

    - 查看所有 git 命令和配置：git help

      - git help -a： list available subcommands

        - ![](https://api2.mubu.com/v3/document_image/44c4135f-4841-4103-8d96-9f70cde4c975-11568850.jpg)

      - git help -g：some concept guides.

        - - ![](https://api2.mubu.com/v3/document_image/0ae357ce-9645-422d-8d44-e97509c8f299-11568850.jpg)

      - 'git help <command>' or 'git help <concept>': 查看某个命令的手册页或查看某个概念，会在网页上打开 git 相关的安装在本地的文件。

    - 查看某个命令的参数：git 命令 --help|-h
    - 彩色的 git 输出：git config color.ui true（好像没啥区别，似乎原来就是彩色的，设为 false 也没变）
    - 配置显示历史记录时，每个提交的信息只显示一行：git config format.pretty oneline；之后怎么变回去呢？？
    - 交互式添加文件到暂存区：git add -i

      - git add -i | --interactive // 进入交互式暂存编辑界面

        - ![](https://api2.mubu.com/v3/document_image/b979e5c8-0757-4e23-bc19-1eb3e2e88114-11568850.jpg)
        - ![](https://api2.mubu.com/v3/document_image/80deb3f4-10d5-48d4-8ff6-4ee871934823-11568850.jpg)
        - ![](https://api2.mubu.com/v3/document_image/b9ccf6bb-0e2f-44ac-bded-55041776926b-11568850.jpg)
        - ![](https://api2.mubu.com/v3/document_image/4aab5f1d-5e09-4c04-839e-e9d22afe4ccb-11568850.jpg)

### .git 文件夹介绍：版本仓库，git 最核心的东西
### Git 三种对象（commit、tree、blob）彼此间的关系：

    - ![](https://api2.mubu.com/v3/document_image/7641ada4-2e3c-4d76-9126-e1b59bb911fd-11568850.jpg)
    - 一个 commit 对应一颗树，这棵树代表这次提交的视图快照

### 分离头指针介绍：detached HEAD

  - 是指我们的变更没有基于某个分支做
  - 那么在做分支切换的时候，这个在分离头指针中做的修改和提交很可能会被 git 当垃圾丢掉
  - 怎么来的，怎么解决：

    - git checkout 基于的分支的分支名：如果这句命令忘了输入最后的这个 checkout 的分支名，就会导致跑到了一个没有分支名称的分离头指针上
    - 然后你改了文件做了提交，那你这个提交在 commit 历史中不会显示出来

      - ![](https://api2.mubu.com/v3/document_image/9a0ab1ea-26de-4b72-9f81-f27d8cf53019-11568850.jpg)

    - git commit -am'...'

      - ![](https://api2.mubu.com/v3/document_image/8baf9852-3d16-4273-a71b-44e52a3a6149-11568850.jpg)

    - git checkout master 会有提示；git log --all --oneline --graph 看不见这次提交，gitk -all 也看不见；可以用 git branch add_style 本次 head 提交的哈希值来给他增加一个分支名称：增加好后 git log --all --oneline --graph 可以看见这次提交了，但是有备注信息没有分支名，gitk -all 上可以看见分支名称及提交；然后可以用 git checkout add_style 切换到这个分支上：

      - ![](https://api2.mubu.com/v3/document_image/f8932d8d-f32d-4129-9d82-f8489926f9ef-11568850.jpg)

      - ![](https://api2.mubu.com/v3/document_image/fa29bad3-907f-494a-bece-f5c25997170c-11568850.jpg)
      - ![](https://api2.mubu.com/v3/document_image/2df0a4f1-a20f-41a8-b526-7e5a6498b668-11568850.jpg)

### HEAD 和 branch 分支、commit 的关系

  - HEAD 不仅仅指向某一个分支的最后一次提交，还可以不跟分支挂钩，处于分离头状态的时候，指代某一个具体的 commit，而不跟任何分支挂钩；
  - 当我们做分支切换的时候，HEAD 是会跟着变化指向新的分支的；
  - git checkout -b fix_readme 具体的分支名|某一次 commit 的哈希值 可以看看这里 HEAD 的变化

    - ![](https://api2.mubu.com/v3/document_image/32ecdea2-8c5d-448e-b0d7-922048a0b6ac-11568850.jpg)
    - ![](https://api2.mubu.com/v3/document_image/a595ae62-3589-4e24-bd49-0cf15e9926e2-11568850.jpg)

  - 归根到底 分支也是落脚到某一个 commit 的；所以 HEAD 最终是落脚于某一次 commit 了；
  - 所以 HEAD 是专有的指代上一次 commit 那么就可以根据 HEAD 做一些操作，比如比较两次 commit 的差异：

    - git diff HEAD HEAD^1 表示比较 HEAD 和他的父亲的差异
    - git diff HEAD HEAD^1^1 表示比较 HEAD 和他的父亲的父亲的差异
    - git diff HEAD HEAD^ 同 git diff HEAD HEAD^1
    - git diff HEAD HEAD^^ 同 git diff HEAD HEAD^1^1 等同于 git diff HEAD HEAD~2

### 组成本地仓库的三棵树介绍：

  - 你的本地仓库由 git 维护的三棵“树”组成：
  - 第一个是你的 工作目录，它持有实际文件；
  - 第二个是 暂存区（Index），它像个缓存区域，临时保存你的改动；
  - 最后是 HEAD，它指向你最后一次提交的结果。
  - 历史版本库呢？？
  - - ![](https://api2.mubu.com/v3/document_image/3552b809-fd1d-4ba8-8c50-fed37c8c14f6-11568850.jpg)

### 推荐开发流程 

- - 1、从 develop 新建开发分支（feature/xxx）

  - - git checkout -b feature/xxx

- - 2、创建一个空的 commit，详细描述提交内容

  - - git commit --allow-empty -m "xxx"

- - 3、开发完成后，rebase 远程主干

  - - git fetch
  - - git rebase origin/develop

- - 4、推到远程

  - \* 首次 git push \-u origin feature/xxx
  - \* 首次后，git push （如果失败，git push -f）

- \*5、 在 gitlab 系统提交 merge request。目标为 develop 分支
- \* 6、gitlab 系统专人审核 merge request，有问题会打标记。直至完成审核合并
- \*\* 注意

  - \*\* 不要用编辑器拉取代码。用命令 fetch + rebase 拉取合并
  - \* 全部操作过程不用 pull merge
  - \* git 操作不要用工具，用原生命令

### git cheat sheet（小抄；备忘单）截图

  - - ![](https://api2.mubu.com/v3/document_image/3d7c0748-2100-4f1f-bd8d-cb53bc0353e5-11568850.jpg)
    - ![](https://api2.mubu.com/v3/document_image/5e8db28b-b941-4bac-885a-e1f096fecd64-11568850.jpg)

### 相关的 linux 操作：

  - ls -al : 显示当前路径下的文件

    - - ![](https://api2.mubu.com/v3/document_image/563cbfa4-b543-46e4-8fa4-6a2a03b8c2d7-11568850.jpg)

  - pwd 命令：显示当前路径

    - pwd

      - 直接使用 pwd 命令，查看当前的工作路径。不加参数默认是与 pwd -L 命令是一致的。

    - pwd -L

      - 显示逻辑路径
      - 其中：L 是取 logical 的首字母，显示当前的路径。如果有连接文件时，直接显示连接文件的路径

    - pwd -p

      - 区别与 pwd -L，参数 p 取自 physical 首字母。主要是如果有连接文件时，不使用连接路径，直接显示连接文件所指向的文件

    - pwd -help

      - 显示 pwd 的在线帮助

    - pwd -version

      - 显示版本信息

  - cat 文件名：cat HEAD 查看文件内容（在命令行里显示出来）
  - vi：查看文件内容（如果不存在则创建一个空格文件）

    - vi index.html

  - i：文件内容查看状态切换成可编辑状态
  - :w! 保存修改
  - :wq! 强制保存修改
  - cd 文件夹路径：进入该文件夹
  - cp：把文件（夹）从一个地方复制到另一个地方（可以重命名）

    - cp 源路径 目标路径
    - cp -r 源路径 .

  - clear：清理屏幕
  - mkdir：新建文件夹

    - mkdir doc

  - rm 文件名：删除某个文件
  - echo 'hi' > doc/readhim: 将 hi 写入这个文件，如果不存在则 readhim 则同时创建
  - mv readme readme.md：将 readme 重命名为 readme.md
