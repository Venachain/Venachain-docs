# Venachain编译

## 系统要求

本文档提供Venachain在Linux环境下安装方式，在编译安装Venachain之前，需确保系统已经安装了如下环境:

-   gcc 7.3+
-   cmake 3.10+
-   go 1.11.4+ （需开启go mod 模式）

下表是单节点的配置要求，您可根据实际业务需求，合理配置机器资源。

| 资源 | 最低配置  | 推荐配置  |
| ---- | --------- | --------- |
| CPU  | 1核1.5GHZ | 4核2.4GHZ |
| 内存 | 1GB       | 8GB       |
| 带宽 | 1Mb       | 10Mb      |

**操作系统要求**

-   CentOS 7.2+
-   Ubuntu16.04+
-   macOS 10.14+

## 源码下载及编译

首先用户需要下载Venachain源码并编译

``` bash
# 获取Venachain源码
git clone https://git-c.i.wxblockchain.com/vena/src/venachain.git

# 编译Venachain
cd Venachain
make clean && make all
```

编译完成后，会在 `release` 目录下生成搭链所需的材料。

如果编译失败，请确保您正确安装了所需的环境，然后重新尝试。

## 清理环境

``` bash
cd ${WORKSPACE}/scripts/
./venachainctl.sh clear -a
```
