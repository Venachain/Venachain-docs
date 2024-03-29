# Venachain部署指南

## 0.  阅读指南

在进行链的部署前，请先阅读完本章 ``0.阅读指南`` 中的内容。**如果存在文档也无法提供帮助解决的问题，或者文档有地方存在异议，请及时联系文档最后一次的修改人。**

### 文档使用方法

- 本部署指南介绍的是Venachain的较基本的一些部署方式，适合想要快速部署Venachain的使用者阅读，使用简单的一行命令就能够完成Venachain的各种方式部署。
- 请先阅读 [**文档《Venachain部署介绍》**](./Venachain部署介绍.md) ，了解关于Venachain部署中涉及的一些名词解释与部署环境的目录结构。
- 关于本文档给出的命令的使用方法、参数说明、使用示例以及注意事项，请参考文档 [**《部署工具venachainctl操作指南》**](../5_实用工具/部署工具venachainctl.md)  。
- 关于Venachain的下载与编译请见 [**文档《Venachain编译》**](../6_深入使用指南/Venachain编译.md) 。

## 1. 单机部署

### 操作目录部署

更加灵活的单机部署方式，请见 [**文档《进阶：单机分步部署》**](./进阶：单机分步部署.md) 。

#### 单节点

```bash
./venachainctl.sh one
```

#### 四节点

```bash
./venachainctl.sh four
```

### 独立目录部署

#### 单节点

```bash
./venachainctl.sh remote deploy -m one -p ${PROJECT_NAME}
```

#### 四节点

```bash
./venachainctl.sh remote deploy -m four -p ${PROJECT_NAME}
```

## 2. 多机部署

### 操作目录部署

请见 [**文档《进阶：多机分步部署》**](./进阶：多机分步部署.md) 。

### 独立目录部署

```{warning}
如果节点部署的机器与操作机不是一台机器，那么操作机需要完成向目标机的免密登录操作，部署过程中，操作机需要对目标机进行ssh操作与scp操作
```

#### 自动生成默认配置进行部署

##### 单个节点

```bash
./venachainctl.sh remote deploy -p ${PROJECT_NAME} -addr ${USERNAME}@${IP_ADDR}
```

##### 多个节点

```bash
./venachainctl.sh remote deploy -p ${PROJECT_NAME} -addr ${USERNAME}@${IP_ADDR},${USERNAME}@${IP_ADDR}
```

#### 根据已有配置文件进行部署

##### 单个节点

```bash
./venachainctl.sh remote deploy -p ${PROJECT_NAME} -n ${NODE_ID}
```

##### 多个节点  

```bash
./venachainctl.sh remote deploy -p ${PROJECT_NAME} -n ${NODE_ID},${NODE_ID}
```

##### 所有节点

```bash
./venachainctl.sh remote deploy -p ${PROJECT_NAME} -a
```

## 3. 配置文件生成

### 自动生成

#### 操作目录部署

```bash
./venachainctl.sh dcgen -n ${NODE_ID}
```

#### 独立目录部署

```{warning}
如果节点部署的机器与操作机不是一台机器，那么操作机需要完成向目标机的免密登录操作，生成文件的过程中，操作机需要对目标机进行ssh操作
```

控制台会输出配置文件生成的目录位置。

```bash
./venachainctl.sh remote prepare -p ${PROJECT_NAME} -addr ${USERNAME}@${IP_ADDR}

## 支持多个节点
./venachainctl.sh remote prepare -p ${PROJECT_NAME} -addr ${USERNAME}@${IP_ADDR},${USERNAME}@${IP_ADDR}
```

### 手动生成

根据需求修改配置文件模板，并放入相应的目录。

目录位置：`scripts/../../deployment_conf/projects/${PROJECT_NAME}`

```bash
## Venachain Node Remote Deploy Configuration File ##

## NODE
deploy_path=
user_name=
ip_addr=127.0.0.1
p2p_port=16791
 
## RPC
rpc_addr=0.0.0.0
rpc_port=6791
rpc_api=db,eth,venachain,net,web3,admin,personal,txpool,iris

## WEBSOCKET
ws_addr=0.0.0.0
ws_port=26791

## WASM_LOG
log_dir=
log_size=67108864

## NODE START
bootnodes=
gcmode=archive
tx_count=1000
tx_global_slots=40960
dbtype=leveldb
lightmode=
extra_options=--debug
pprof_addr=
```

## 4. 向已有链添加新节点

### 操作目录部署

请见 [**文档《进阶：多机分步部署》**](./进阶：多机分步部署.md) 。

### 独立目录部署

```{warning}
如果节点部署的机器与操作机不是一台机器，那么操作机需要完成向目标机的免密登录操作，生成文件的过程中，操作机需要对目标机进行ssh操作
```

#### 自动生成默认配置进行部署

##### 输入部署指令

```bash
./venachainctl.sh remote deploy -p ${PROJECT_NAME} -addr ${USERNAME}@${IP_ADDR}
```

##### 控制台操作

```console
[remote/prepare] ${DEPLOYMENT_CONF_PATH}/projects/test already exists, do you want to cover it? Yes or No(y/n): 
```

选择 ``n``

```console
[remote/prepare] Do you mean you want to create new conf file in exist path? Yes or No(y/n): 
```

选择 ``y``

#### 根据已有配置文件进行部署

将生成的配置文件放入相应的目录后，执行节点部署命令。

```bash
./venachainctl.sh remote deploy -p ${PROJECT_NAME} -n ${NODE_ID}
```

## 5. 节点清理

### 操作目录部署

```bash
## 重启节点
./veanchainctl.sh restart -n ${NODE_ID}

## 停止节点
./veanchainctl.sh stop -n ${NODE_ID}

## 链上删除节点
./venachainctl.sh delete -n ${NODE_ID}

## 清除节点
./veanchainctl.sh clear -n ${NODE_ID}

## 上述指令都支持使用 "-a" 代替 "-n ${NODE_ID}" 以对所有节点完成操作
./venachainctl.sh clear -a
```

### 独立目录部署

```bash
## 重启节点
./veanchainctl.sh remote clear -m restart -p ${PROJECT_NAME} -n ${NODE_ID}

## 停止节点
./veanchainctl.sh remote clear -m stop -p ${PROJECT_NAME} -n ${NODE_ID}

## 链上删除节点
./venachainctl.sh remote clear -m delete -p ${PROJECT_NAME} -n ${NODE_ID}

## 清除节点
./veanchainctl.sh remote clear -m clean -p ${PROJECT_NAME} -n ${NODE_ID}

## 上述指令都支持使用 "-a" 代替 "-n ${NODE_ID}" 以对所有节点完成操作
./venachainctl.sh remote clear -m clean -p ${PROJECT_NAME} -a
```
