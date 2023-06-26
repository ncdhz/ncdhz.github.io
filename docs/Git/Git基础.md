# Git基础

1. git是版本管理工具
2. git数据完整性
   + 在保存到 Git 之前，所有数据都要进行内容的校验和（checksum）计算，并将此结果作为数据的唯一标识和索引。换句话说，不可能在你修改了文件或目录之后，Git 一无所知。
   + 所以如果文件在传输时变得不完整，或者磁盘损坏导致文件数据缺失，Git 都能立即察觉。
   + Git 使用 SHA-1 算法计算数据的校验和，通过对文件的内容或目录的结构计算出一个 SHA-1 哈希值，作为指纹字符串。该字串由 40 个十六进制字符（0-9 及 a-f）组成。
3. Git 工作流程
   + 在工作目录中修改某些文件。
   + 对修改后的文件进行快照，然后保存到暂存区域。
   + 提交更新，将保存在暂存区域的文件快照永久转储到 Git 目录中。
4. 安装git
    + 从源代码安装
        + Git 的工作需要调用 curl，zlib，openssl，expat，libiconv 等库的代码，所以需要先安装这些依赖工具。在有 yum 的系统上（比如 Fedora）或者有 apt-get 的系统上（比如 Debian 体系），可以用下面的命令安装：  

            ```shell
            yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
            apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
            ```

        + 下载源码：`https://github.com/git/git/releases`
        + 编译并安装：  

            ```shell
            tar -zxf git-1.7.2.2.tar.gz
            cd git-1.7.2.2
            make prefix=/usr/local all
            sudo make prefix=/usr/local install
            ```

    + 在 Linux 上安装  
        + 在 Fedora 上用 yum 安装：`yum install git-core`  
        + 在 Ubuntu 这类 Debian 上用 apt-get 安装：`apt-get install git`
    + 在 Mac 上安装  `sudo port install git-core +svn +doc +bash_completion +gitweb`
    + 在 Windows 上安装 [https://gitforwindows.org](https://gitforwindows.org) 直接下载exe安装
5. 初次运行 Git 前的配置
    + 用户信息配置

        ```shell
        git config --global user.name 用户名
        git config --global user.email 邮箱
        ```

6. 配置信息查看  `git config --list`
7. 查看单独的配置信息如 user.name  `git config user.name`
8. 帮助  

   ```shell
    git help <verb>
    git <verb> --help
    man git-<verb>
   ```

9. 工作目录中初始化新仓库  `git init`
10. 告诉 Git 开始对这些文件进行跟踪  `git add  文件名或通配符`
11. 克隆项目到本地  `git clone https://github.com/git/git.git`
12. 检查当前文件状态`git status`
13. 提交更新  `git commit`
14. 移除文件  `git rm`
15. 移动文件  `git mv`
16. 查看提交历史  `git log`
17. 撤销操作  `git commit --amend`
18. 添加远程仓库  `git remote -v origin  git@github.com:git/git.git`
19. 推送数据到远程仓库  `git push -u origin master`
20. 查看远程仓库信息  `git remote show [remote-name]`
21. 抓取数据合并到本地  `git pull`
22. 删除或重命名远程仓库  `git remote rename 原先的名字 现在的名字`  `git remote rm 名字`
