# Centos7的基础知识

1. 查看ip地址 `ip addr`
2. `.tar.gz`包解压 `tar -zxvf 路径`
3. 递归创建文件夹 `mkdir -p 路径`
4. 防火墙
    + 关闭防火墙 `systemctl stop firewalld.service`
    + 开启防火墙 `systemctl start firewalld.service`
    + 禁止开机启动 `systemctl disable firewalld.service`
    + 查看防火墙状态 `firewall-cmd --state`

5. `scp`拷贝

   ```shell
    scp -r 文件所在目录 用户名@地址:接收文件目录
    如：scp -r /etc root@192.168.1.1:/etc/hosts
    scp -r 用户名@地址:文件所在目录 接收文件目录  
    如：scp -r root@192.168.1.1:/etc/hosts /etc 
    -r 表示对文件夹递归拷贝
   ```

6. `ssh` 免密登入
   + 假设有两台主机主机的`ip`

        ```
        192.168.56.201
        192.168.56.202
        ```

   + 生成密匙 `ssh-keygen -t rsa`
   + 复制公钥

        ```shell
        在 192.168.56.201 主机中执行
        ssh-copy-id -i /root/.ssh/id_rsa.pub 192.168.56.202
        在 192.168.56.202 主机中执行
        ssh-copy-id -i /root/.ssh/id_rsa.pub 192.168.56.202
        在 192.168.56.202 主机中执行
        scp -r /root/.ssh/authorized_keys root@192.168.56.201:/root/.ssh
        ```

   + 现在免密登入就配置好了可以尝试 `ssh 192.168.56.201` 连接一下
