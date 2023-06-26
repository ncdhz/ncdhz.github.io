# Mac Docker版 GitLab 安装

1. 安装Docker
2. 下载镜像  `docker pull gitlab/gitlab-ce`
3. 运行 Gitlab 镜像

    ```shell
    # 通过docker run中加入环境变量,取名为gitlab
    docker run --detach \       # 后台运行 -d
    -p 8443:443 \           # 将容器内443端口映射到主机8443,提供https服务
    -p 8888:80 \              # 将容器内80端口映射到主机8888,提供http服务
    -p 10022:22 \           # 将容器内22端口映射到主机10022,提供ssh服务
    --name gitlab \         # 指定容器名称
    --restart=unless-stopped \                   # 容器运行中退出时（不是手动退出）,自动重启
    --volume /Users/majunlong/gitlab/etc:/etc/gitlab \       # 将本地/Users/majunlong/gitlab/etc挂载到容器内/etc/gitlab
    --volume /Users/majunlong/gitlab/log:/var/log/gitlab \   # 将本地将本地/Users/majunlong/gitlab/log挂载到容器内/var/log/gitlab
    --volume /Users/majunlong/gitlab/data:/var/opt/gitlab \  # 将本地将本地/Users/majunlong/gitlab/data挂载到容器内/var/opt/gitlab
    gitlab/gitlab-ce                # 镜像名称:版本
    ```

    + 删除描述文字和换行符号的命令

        ```shell
        docker run --detach -p 8443:443 -p 8888:80 -p 10022:22  --name gitlab --restart=unless-stopped --volume /Users/majunlong/gitlab/etc:/etc/gitlab  --volume /Users/majunlong/gitlab/log:/var/log/gitlab --volume /Users/majunlong/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce
        ```

4. 通过连接[http://127.0.0.1:8888](http://127.0.0.1:8888) 进入Gitlab的网站账号：`root` 密码：`root`
