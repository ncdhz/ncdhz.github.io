# Ubuntu Anaconda 下载

1. 下载 `python 3.7` 版本的 `anaconda`

    ```shell
    wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2019.10-Linux-x86_64.sh
    ```

2. 安装 `anaconda`

    ```shell
    sudo sh ./Anaconda3-2019.10-Linux-x86_64.sh
    ```

3. 配置环境变量

    ```shell
    vi /etc/profile
    # 添加下面代码
    export PATH=$PATH:(Anaconda to path)/bin
    source /etc/profile
    # 重启电脑所有用户生效
    ```
