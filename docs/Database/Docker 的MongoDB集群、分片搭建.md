# Docker 的MongoDB集群、分片搭建

## MongoDB 分片集群组成

1. shard 官方建议采用副本集，提供数据冗余和高可用，主要存储业务数据
2. mongos 是应用程序的接口，通过它，应用程序与整个集群是透明的，故一般每个应用服务器对应一个实例，可以跟应用部署到一台服务器上。它主要读取或缓存配置服务器中元数据，提供查询路由到每个分片的功能
3. configure servers 官方建议采用副本集，存储集群的元数据。很重要，能够影响集群的使用

## 创建 ip 容器

```shell
sudo docker network create --subnet=172.16.0.0/16 mongo-net
```

## Docker 下载 MongoDB

```shell
docker pull mongo
```

## 创建配置服务的副本集

1. 新建配置文件保存目录

    ```shell
    # 主要映射 docker 中的配置文件便于在主机端管理
    ~/mongodb/data/cs/configsvr0
    ~/mongodb/data/cs/configsvr1
    ~/mongodb/data/cs/configsvr2
    ```

2. 创建副本集

    ```shell
    docker run -d --name configsvr0 --net mongo-net --ip 172.16.0.2 -v ~/mongodb/data/cs/configsvr0:/data/configdb mongo --configsvr --replSet "rs_configsvr" --bind_ip_all
    docker run -d --name configsvr1 --net mongo-net --ip 172.16.0.3 -v ~/mongodb/data/cs/configsvr1:/data/configdb mongo --configsvr --replSet "rs_configsvr" --bind_ip_all
    docker run -d --name configsvr2 --net mongo-net --ip 172.16.0.4 -v ~/mongodb/data/cs/configsvr2:/data/configdb mongo --configsvr --replSet "rs_configsvr" --bind_ip_all
    ```

3. 通过 docker inspect 容器名去获取在容器里的ip地址

    ```shell
    docker inspect configsvr0 | grep IPAddress
    docker inspect configsvr1 | grep IPAddress
    docker inspect configsvr2 | grep IPAddress
    ```

    + ip 信息

        ```shell
        # 后面端口为默认端口
        configsvr0: 172.16.0.2:27019
        configsvr1: 172.16.0.3:27019
        configsvr2: 172.16.0.4:27019
        ```

4. 初始化配置服务副本集
    + 进入 configsvr0

        ```shell
        docker exec -it configsvr0 bash
        ```

    + 进入 MongoDB 终端

        ```shell
        mongo --host 172.16.0.2 --port 27019
        ```

    + 在 MongoDB 终端执行下面的命令

        ```javascript
        rs.initiate(
            {
                _id: "rs_configsvr",
                configsvr: true,
                members: [
                    {   
                        _id : 0, 
                        host : "172.16.0.2:27019" 
                    },
                    { 
                        _id : 1, 
                        host : "172.16.0.3:27019" 
                    },
                    {   
                        _id : 2, 
                        host : "172.16.0.4:27019"
                    }
                ]
            }
        )
        ```

## 创建分片副本集

1. 新建配置文件保存目录

    ```shell
    ~/mongodb/data/sh/shardsvr00
    ~/mongodb/data/sh/shardsvr01
    ~/mongodb/data/sh/shardsvr02
    ~/mongodb/data/sh/shardsvr10
    ~/mongodb/data/sh/shardsvr11
    ~/mongodb/data/sh/shardsvr12
    ```

2. 创建副本集

    ```shell
    docker run --name shardsvr00 -d --net mongo-net --ip 172.16.0.5 -v ~/mongodb/data/sh/shardsvr00:/data/db mongo --shardsvr --replSet "rs_shardsvr0" --bind_ip_all
    docker run --name shardsvr01 -d --net mongo-net --ip 172.16.0.6 -v ~/mongodb/data/sh/shardsvr01:/data/db mongo --shardsvr --replSet "rs_shardsvr0" --bind_ip_all
    docker run --name shardsvr02 -d --net mongo-net --ip 172.16.0.7 -v ~/mongodb/data/sh/shardsvr02:/data/db mongo --shardsvr --replSet "rs_shardsvr0" --bind_ip_all
    docker run --name shardsvr10 -d --net mongo-net --ip 172.16.0.8 -v ~/mongodb/data/sh/shardsvr10:/data/db mongo --shardsvr --replSet "rs_shardsvr1" --bind_ip_all
    docker run --name shardsvr11 -d --net mongo-net --ip 172.16.0.9 -v ~/mongodb/data/sh/shardsvr11:/data/db mongo --shardsvr --replSet "rs_shardsvr1" --bind_ip_all
    docker run --name shardsvr12 -d --net mongo-net --ip 172.16.0.10 -v ~/mongodb/data/sh/shardsvr12:/data/db mongo --shardsvr --replSet "rs_shardsvr1" --bind_ip_all
    ```

3. 通过 docker inspect 容器名去获取在容器里的ip地址

    ```shell
    docker inspect shardsvr00 | grep IPAddress
    docker inspect shardsvr01 | grep IPAddress
    docker inspect shardsvr02 | grep IPAddress
    docker inspect shardsvr10 | grep IPAddress
    docker inspect shardsvr11 | grep IPAddress
    docker inspect shardsvr12 | grep IPAddress
    ```

    + ip 信息

        ```shell
        # 由于–shardsvr 的默认端口为 27018
        shardsvr00: 172.16.0.5:27018
        shardsvr01: 172.16.0.6:27018
        shardsvr02: 172.16.0.7:27018

        shardsvr10: 172.16.0.8:27018
        shardsvr11: 172.16.0.9:27018
        shardsvr12: 172.16.0.10:27018
        ```

4. 配置分片副本集
    + 配置 shardsvr00

        ```shell
        # 进入 shardsvr00
        docker exec -it shardsvr00 bash
        # 进入 MongoDB 终端
        mongo --host 172.16.0.5 --port 27018
        # 配置
        rs.initiate(
            {
                _id : "rs_shardsvr0",
                members: [
                    { 
                        _id : 0, 
                        host : "172.16.0.5:27018" 
                    },
                    { 
                        _id : 1, 
                        host : "172.16.0.6:27018"
                    },
                    { 
                        _id : 2, 
                        host : "172.16.0.7:27018" 
                    }
                ]
            }
        )
        ```

    + 配置 shardsvr10

        ```shell
        # 进入 shardsvr10
        docker exec -it shardsvr10 bash
        # 进入 MongoDB 终端
        mongo --host 172.16.0.8 --port 27018
        # 配置
        rs.initiate(
            {
                _id : "rs_shardsvr1",
                members: [
                    { 
                        _id : 0, 
                        host : "172.16.0.8:27018" 
                    },
                    { 
                        _id : 1, 
                        host : "172.16.0.9:27018"
                    },
                    { 
                        _id : 2, 
                        host : "172.16.0.10:27018" 
                    }
                ]
            }
        )
        ```

## 创建mongos

```shell
docker run --name mongos0 --net mongo-net --ip 172.16.0.11 -d -p 27017:27017 --entrypoint "mongos" mongo --configdb rs_configsvr/172.16.0.2:27019,172.16.0.3:27019,172.16.0.4:27019 --bind_ip_all
```

1. 查看地址

    ```shell
    docker inspect mongos0 | grep IPAddress
    172.16.0.11:27017
    ```

2. 进入 mongos0 并进入 mongo

    ```shell
    docker exec -it mongos0 bash
    mongo --host 172.16.0.11 --port 27017   
    ```

3. 添加分片到集群

    ```javascript
    sh.addShard("rs_shardsvr0/172.16.0.5:27018,172.16.0.6:27018,172.16.0.7:27018")
    sh.addShard("rs_shardsvr1/172.16.0.8:27018,172.16.0.9:27018,172.16.0.10:27018")
    ```

4. 数据库启用分片

    ```javascript
    sh.enableSharding("test")
    ```

5. 分片集合

    ```javascript
    sh.shardCollection("test.order", {"_id": "hashed" })
    ```

## 测试

1. 插入数据

    ```javascript
    // 选择数据库
    // use test
    // 利用 for 插入数据
    for (i = 1; i <= 1001; i=i+1){
        db.order.insert({
            'test': 1
        })
    }
    ```

2. 查看数据分布

    ```javascript
    db.order.find().count()
    ```

3. 进入分片的库查看数据分布

    ```javascript
    # 进入 0 片
    docker exec -it shardsvr00 bash
    mongo --host 172.16.0.5 --port 27018
    db.order.find().count()
    # 进入 1 片
    docker exec -it shardsvr10 bash
    mongo --host 172.16.0.8 --port 27018
    db.order.find().count()
    ```
