# Centos 新建用户并授权

## 创建新用户

1. 创建新用户 `n`

    ```shell
    # adduser 用户名
    adduser n
    ```

2. 为 `n` 初始化密码

    ```shell
    passwd n
    ```

## 授权

由于新用户不能使用 `sudo` 所以需要给他授权

1. 查看 `sudoers` 文件权限

    ```shell
    ls -l /etc/sudoers
    ```

2. 修改 `sudoers` 权限

    ```shell
    # 如果查看的权限如下，就需要修改权限
    # -r--r----- 1 root root 3938 Sep  6  2017 /etc/sudoers 只读权限
    # 添加写权限
    chmod -v u+w /etc/sudoers
    ```

3. 修改 `sudoers` 文件

    ```shell
    ## Allow root to run any commands anywhere 
    root ALL=(ALL) ALL
    # 这是新添加的
    n    ALL=(ALL) ALL
    ```

4. 收回权限

    ```shell
    chmod -v u-w /etc/sudoers
    ```
