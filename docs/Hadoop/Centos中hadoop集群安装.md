# Centos中hadoop集群安装

1. 安装环境
    + `centos7` 3台
    + `java1.8`
    + `hadoop2.7.7`

2. `centos7` 安装省略（我用的是`VirtualBox`虚拟机）  
3. `centos7` 基本配置
    + 配置静态ip  

        ```
        192.168.56.201
        192.168.56.202
        192.168.56.203
        ```

    + 配置hostname
        + 192.168.56.201 这台 `hostname centos1`
        + 192.168.56.202 这台 `hostname centos2`
        + 192.168.56.203 这台 `hostname centos3`
  
    + 配置 `hosts`

        ```shell
        对于第一台
        vi /etc/hosts
        添加
        192.168.56.201 centos1
        192.168.56.202 centos2
        192.168.56.203 centos3
        scp -r /etc/hosts root@centos2:/etc/hosts
        scp -r /etc/hosts root@centos3:/etc/hosts
        ```

    + 配置 `ssh` [免密登入](/2019/05/05/centos7的基本操作.html#ssh)
        + 在 centos1 中  

            ```shell
            ssh-copy-id -i /root/.ssh/id_rsa.pub centos3
            ```

        + 在 centos2 中  

            ```shell
            ssh-copy-id -i /root/.ssh/id_rsa.pub centos3
            ```

        + 在 centos3 中  

            ```shell
            ssh-copy-id -i /root/.ssh/id_rsa.pub centos3
            scp -r authorized_keys root@centos1:/root/.ssh/
            scp -r authorized_keys root@centos2:/root/.ssh/
            ```

4. `java`虚拟机安装省略（hadoop是基于java写的所以必须安装java虚拟机）
5. 下载 [hadoop](https://hadoop.apache.org/releases.html)
6. 安装 `hadoop`
    + 安装目录简介 `hadoop 我安装在 /usr/local/目录下`
    + 解压 `tar -zxvf  hadoop-2.7.7.tar.gz`
    + 配置hadoop `cd hadoop-2.7.7/etc/hadoop`  
        + 配置 `hadoop-env.sh`  

            ```shell
            vi hadoop-env.sh
            export JAVA_HOME=你的JAVA_HOME路径
            ```

        + 配置 `core-site.xml`  

            ```shell
            创建hadoop临时文件路径
            mkdir -p /usr/local/hadoop-2.7.7/data/tmp
            ```

            ```xml
            <configuration>
                <!-- 指定HDFS的通信地址 -->
                <property>
                <name>fs.defaultFS</name>
                <value>hdfs://centos1:9000</value>
                </property>
                <!-- 指定hadoop运行时产生文件的存储路径 -->
                <property>
                <name>hadoop.tmp.dir</name>
                <value>/usr/local/hadoop-2.7.7/data/tmp</value>
                </property>
            </configuration>
            ```

        + 配置 `hdfs-site.xml`  

            ```shell
            创建 name 存放目录和 data 存放目录
            mkdir -p /usr/local/hadoop-2.7.7/data/name
            mkdir -p /usr/local/hadoop-2.7.7/data/data
            ```

            ```xml
            <configuration>
                <!-- 指定hadoop namenode 节点初始化数据目录 -->
                <property>
                <name>dfs.namenode.name.dir</name>
                <value>/usr/local/hadoop-2.7.7/data/name</value>
                </property>
                <!-- 指定hadoop datanode 节点数据目录 -->
                <property>
                <name>dfs.datanode.data.dir</name>
                <value>/usr/local/hadoop-2.7.7/data/data</value>
                </property>
                <!-- 指定hadoop datanode数据备份数量根据自己机器配置默认保存三份 -->
                <property>
                <name>dfs.replication</name>
                <value>3</value>
                </property>
                <!-- 指定sencondary namenode通信地址可以使用默认 -->
                <property>
                <name>dfs.namenode.secondary.http-address</name>
                <value>centos1:50090</value>
                </property>
            </configuration>
            ```

        + 配置 `mapred-site.xml`  

            ```xml
            cp mapred-site.xml.template mapred-site.xml
            <configuration>
                <!-- 指定map reduce运行框架-->
                <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
                </property>
            </configuration>
            ```

        + 配置 `yarn-site.xml`  

            ```xml
            <configuration>
                <!-- resourcemanager 的地址-->
                <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>centos1</value>
                </property>
                <!-- 指定reducer获取数据的方式-->
                <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
                </property>
            </configuration>
            ```

        + 配置 `slaves` (datanode 的机器)  

            ```
            添加
            centos1
            centos2
            centos3
            ```

        + 配置环境变量  

            ```shell
            export HADOOP_HOME=/usr/local/hadoop-2.7.7
            export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
            ```

        + 拷贝hadoop环境到另外的两台机器

            ```shell
            scp -r /usr/local/hadoop-2.7.7/ root@centos2:/usr/local/
            scp -r /usr/local/hadoop-2.7.7/ root@centos3:/usr/local/
            scp -r /etc/profile root@centos2:/etc/
            scp -r /etc/profile root@centos3:/etc/
            ```

        + 初始化 `namenode`  

            ```shell
            hadoop  namenode  -format
            ```

        + 在centos1中启动 `hdfs` 和 `yarn`

            ```shell
            start-dfs.sh
            start-yarn.sh
            ```

        + 关闭[防火墙](/2019/05/05/centos7的基本操作.html#firewalld)

            ```shell
            systemctl stop firewalld.service
            systemctl disable firewalld.service 
            ```

        + 访问 `hdfs` 管理网页 <http://centos1:50070>  
        + 访问 `yarn` 管理网页 <http://centos1:8088> 

7. 测试
    + 测试 `hdfs`
        + 新建一个文件 `echo "hello hadoop word" >> hello_hadoop`
        + 上传数据到`hdfs` `hadoop fs -put hello_hadoop /`  
        + 从`hdfs`中获取文件 `hadoop fs -get /hello_hadoop`

    + 测试 `MapReduce`  
        + `cd /usr/local/hadoop-2.7.7/share/hadoop/mapreduce`
        + 运行`pi`测试程序`hadoop jar  hadoop-mapreduce-examples-2.7.7.jar pi 5 5`  
