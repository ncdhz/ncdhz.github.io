# Ubuntu 防火墙相关操作

1. 开启防火墙

    ```shell
    sudo ufw enable
    ```

2. 关闭防火墙

    ```shell
    sudo ufw disable
    ```

3. 重启防火墙

    ```shell
    sudo ufw reload
    ```

4. 查看防火墙状态

    ```shell
    sudo ufw status
    ```

5. 开放端口

    ```shell
    # 开放 80 端口
    sudo ufw allow 80
    ```

5. 关闭端口

    ```shell
    # 关闭 80 端口
    sudo ufw deny 80
    ```
