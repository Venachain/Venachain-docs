# VenaProof 安装

| **时间**   | **修改人** | **修改事项** | **存证平台版本** | **文档版本** |
| ---------- | ---------- | ------------ | ---------------- | ------------ |
| 2022.05.20 | 吴经文     | 初稿         | 0.0.1            | 1.0          |

```{warning}
仅支持linux
```

## 环境准备

### 硬件环境准备

- 主 机：Intel Xeon E5-2650或以上。
- 内 存：16G或以上。
- 硬 盘：32GB或以上。
- 图形卡：VGA/DVI。

### 软件环境准备

- OS Linux 64bit
- Golang 1.14.4 或以上
- Venachain v1.0.0 或以上
- MongoDB 4.2.8 或以上
- VenaProof v0.0.1

## 安装步骤

### 1. 安装和启动MongoDB

1. 下载地址：[MongoDB下载](https://www.mongodb.com/try/download/community)，下载tgz格式

2. 安装

    ```bash
    ## 解压
    tar -zxvf mongodb-xxxx.tgz

    ## 移动
    mv mongodb-xxxx /usr/local/mongodb
    ```

3. 配置环境变量

    添加以下内容并source配置文件

    ```bash
    export MONGODB=/usr/local/mongodb
    export PATH=$PATH:$MONGODB/bin
    ```

4. 新建目录

    ```bash
    mkdir -p /usr/local/mongodb/db
    mkdir -p /usr/local/mongodb/log
    ```

5. 启动

    ```bash
    mongod -dbpath /usr/local/mongodb/db -logpath /usr/local/mongodb/log --logappend --port 27017 --fork
    ```

    ```{note}
    如果启动失败，可以查看 `/usr/local/mongodb/log` 下的日志进行排查
    ```

### 2. 配置MongoDB

1. 进入mongodb

    ```bash
    mongo
    ```

2. 新建数据库

    ```console
    use ${db_name}
    ```

3. 新建账户

    ```console
    db.createUser(
        {
            user: "${user_name}",
            pwd: "${password}",
            roles: [ "readWrite", "${db_name}" ]
        }
    );
    ```

### 3. 部署Venachain

1. 下载

    获取venachain可执行文件，下载地址：[venachain可执行文件下载](https://git-c.i.wxblockchain.com/vena/src/venachain/-/tags)

2. 部署

    请参考 [Venachain部署文档](../../2_区块链部署/Venachain部署指南1.md) 进行链的部署

### 4. 安装VenaProof

1. 下载

    获取venaproof可执行文件，下载地址：[venaproof可执行文件下载](https://git-c.i.wxblockchain.com/vena/src/venaproof/-/tags)

2. 解压到部署目录：

    ```bash
    tar -zxvf VenaProof_integration.XXXX.tar.gz -C ${deploy_path}
    ```

### 5. 配置VenaProof

1. 新建配置文件

    ```bash
    cd ${deploy_path}/release/conf
    cp config.toml.template config.toml
    ```

2. 根据mongodb与venachain的情况，填入配置信息

    ```toml
    [http]
    ip = "${venaproof部署的机器的ip}"
    port = "17017"
    mode = "debug"

    [mongodb]
    ip = "${mongodb部署的机器的ip}"
    port = "27017"
    username = "${user_name}"
    password = "${password}"
    dbname = "${db_name}"

    [venachain]
    ip = "${firstnode_ip}"
    rpc_port = "${firstnode_rpcport}"
    passphrase = "${firstnode_account_phrase}"

    [ws]
    buff_size = 12
    ```

### 6. 启动VenaProof

1. 启动

    ```bash
    cd ${deploy_path}/release/scripts
    ./vpctl.sh start
    ```

2. 关闭

    ```bash
    cd ${deploy_path}/release/scripts
    ./vpctl.sh stop
    ```

3. 清理

    ```bash
    cd ${deploy_path}/release/scripts
    ./vpctl.sh clean
    ```

    ```{note}
    清理命令会关闭服务并删除 `data` 目录下的数据，而 `conf` 目录下的 `config.toml` 不会删除，若需要删除请手动操作。
    ```
