# Docker中安装mysql

1. 下载并安装[Docker](https://www.docker.com/get-started)
2. 执行命令搜索`mysql` `docker search mysql`
3. 安装 `mysql` `docker pull mysql`
4. 运行 `mysql`

    ```shell
    docker run -p 3308:3306 --name mysql -e MYSQL_ROOT_PASSWORD=root -d mysql  
    -p 3308:3306 :将容器的3306端口映射到主机的3308端口。    
    -e MYSQL_ROOT_PASSWORD=root：初始化 root 用户的密码。
    ```

5. 找到容器的 `id` `docker ps -a` 【id】
6. 进入容器 `docker exec -it （刚才找到的id） /bin/bash`
7. 进入`mysql` `mysql -uroot -p` 输入刚才的密码
8. 授权 `GRANT ALL ON *.* TO 'root'@'%';`
9. 刷新权限 `flush privileges;`
10. 更新加密规则 `ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;`
11. 更新root用户密码 `ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';`
12. 刷新权限 `flush privileges;`

## 如果需要删除 `ONLY_FULL_GROUP_BY` 进行如下操作

1. 执行 `select @@sql_mode;`
2. 复制除 `ONLY_FULL_GROUP_BY` 以外的部分
3. 执行

    ```shell
    set GLOBAL sql_mode = 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';
    ```  

4. 不过以上操作数据库重启后就会失效，以下操作能够全局的删除`ONLY_FULL_GROUP_BY`
5. 找到当前容器中的文件 `/etc/mysql/conf.d/docker.cnf`（可以用docker命令copy到本地，修改完成后上传。`docker cp 容器id:/etc/mysql/conf.d/docker.cnf 本地路径` `docker cp 本地路径/docker.cnf 容器id:/etc/mysql/conf.d/`）
6. 在最后一行加入

    ```shell
    sql_mode = STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
    (参数部分如上是执行select @@sql_mode;语句复制除ONLY_FULL_GROUP_BY 以外的部分)
    ```
