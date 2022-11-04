# Wasm合约开发工具Venahcain-CDT

**Venachain-CDT** 是WebAssembly(WASM)工具链和 **Venachain** 平台智能合约开发工具集。主要功能如下：

-   创建C++合约模版；
-   将C++合约编译为wasm格式文件，并生成对应合约的ABI文件。

```{note}
安装或使用过程中遇到问题可以阅读 [**Q&A**](cdt_qa) 部分进行排查。
```

```{note}
Windows用户在安装与使用的过程中，建议使用 Git Bash 进行操作。
```

## 安装

有两种安装方式：

-   方式一：使用已编译版本。将本工具对应操作系统的 [**release包**]**(https://github.com/Venachain/Venachain-CDT/releases/tag/v1.0.0) 解压到 `${指定安装目录}` 下。 **Linux** 与 **MacOS** 操作系统用户的 `${指定安装目录}` 为 `/usr/local/` ， **Windows** 用户的 `${指定安装目录}` 为 `C:/` 。
-   方式二：手动编译安装。按照下文 [**编译步骤**](build) 进行手动编译安装。

```{note}
由于方式二的编译时间可能较长，推荐使用方式一。
```

在安装完成后，将 `${指定安装目录}/venachain.cdt` 添加至环境变量中。如果是windows系统，还需要将 `${指定安装目录}/venachain.cdt/bin` 添加至环境变量中。

(build)=

## 编译

### 编译要求

#### Linux

-   Git
-   Python
-   CMake 3.17+
-   gcc&g++ 7.4+ 或 Clang 7.0+

#### MacOS

-   Git
-   Python
-   CMake 3.17+
-   gcc&g++ 7.4+ 或 Clang 7.0+

#### Windows

-   Git
-   Python
-   CMake 3.5+
-   [**MinGW-W64 GCC-8.1.0**](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/sjlj/x86_64-8.1.0-release-posix-sjlj-rt_v6-rev0.7z) （注：安装路径不能含有空格，即不能安装在 `Program Files` 或
    `Program Files(x86)目录` ，否则可能导致编译失败。）

### 编译流程

#### Linux

##### 安装依赖

-   Ubuntu

``` bash
sudo apt install build-essential cmake libz-dev libtinfo-dev libncurses5-dev
```

-   CentOS

``` bash
sudo yum install gcc g++ make ncurses-devel zlib-devel
```

##### 获取源码

``` bash
git clone https://github.com/Venachain/Venachain-CDT.git
```

##### 执行编译

``` bash
cd Venachain-CDT
mkdir build && cd build
cmake .. 
make && make install
```

#### MacOS

##### 获取源码

``` bash
git clone https://github.com/Venachain/Venachain-CDT.git
```

##### 执行编译

``` bash
cd Venachain-CDT
mkdir build && cd build
cmake ..
make && make install
```

#### Windows

##### 获取源码

``` bash
git clone https://github.com/Venachain/Venachain-CDT.git
```

##### 执行编译

``` bash
cd Venachain-CDT

mkdir build && cd build

cmake -G "MinGW Makefiles" .. -DCMAKE_INSTALL_PREFIX="C:/venachain.cdt" -DCMAKE_MAKE_PROGRAM=mingw32-make

mingw32-make && mingw32-make install
```

## 使用

在使用Venachain-CDT之前，须将 `${指定安装目录}/venachain.cdt` 添加至环境变量中。

C++合约项目分为两种类型：

-   单文件项目：即合约项目中只包含一个C++合约文件。
-   多文件项目：若合约文件涉及引用其他多个C++文件，则需要使用该类型。该类型项目采用 `makefile` 方式将合约编译为wasm文件。

### 单文件项目

#### 初始化项目

``` bash
venachain-init -project=${PROJECT_NAME} -bare

## 例
venachain-init -project=example -bare
```

#### 编译WASM文件

``` bash
cd ${PROJECT_NAME}
venachain-cpp -o ${WASM_NAME}.wasm ${CPP_NAME}.cpp -abigen

## 例
cd example
venachain-cpp -o example.wasm example.cpp -abigen
```

### 多文件项目

#### 初始化项目

```bash
venachain-init -project=\${PROJECT_NAME}

# 例 
venachain-init -project=cmake_example
```

#### 编译WASM文件

-   Linux

``` bash
cd ${PROJECT_NAME}/build
cmake ..
make
```

-   MacOS

``` bash
cd ${PROJECT_NAME}/build
cmake ..
make
```

-   Windows

``` bash
cd ${PROJECT_NAME}/build
cmake .. -G "MinGW Makefiles" -DVENACHAIN_CDT_ROOT="C:/venachain.cdt"
mingw32-make
```

(cdt_qa)=
## Q&A

1. 遇到以下问题：

    ``` console
    venachain-init: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by venachain-init)
    venachain-init: /lib64/libstdc++.so.6: version `CXXABI_1.3.9' not found (required by venachain-init)
    venachain-init: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by venachain-init)
    ```

    **原因与解决方法**：gcc和g++版本太低导致，请升级版本。

2. 遇到以下问题：

    ``` console
    can not find libtinfo/ can not fond libncurses
    ```

    **原因与解决方法**：首先确认有没有安装 `libncurses` ，在 `/usr/lib64/` 下：

    -   如果没有，执行

    ``` bash
    ## Ubuntu
    apt -y install lib32ncurses5

    ## CentOS
    yum -y install libncurses.so.5
    ```

    -   若有，则执行

    ``` bash
    ln -s libncurses.so  libncurses.so.5.9
    ```

3. MacOS系统版本比较高时，需要升级boost。

    例如 MacOS Big Sur 版本 11.6.1， `CMakeModules/BoostExternalProject.txt` 的 `ExternalProject_Add` 模块的内容改为：

    ``` console
    ExternalProject_Add(
        boost
        URL https://sourceforge.net/projects/boost/files/boost/1.77.0/boost_1_77_0.tar.bz2/download
        URL_MD5 09dc857466718f27237144c6f2432d86
        CONFIGURE_COMMAND ./bootstrap.sh --with-toolset=clang --with-libraries=filesystem,system,log,thread,random,exception
        BUILD_COMMAND ./b2 link=static ${BUILD_COMMAND_EXTRA}
        BUILD_IN_SOURCE 1
        INSTALL_COMMAND ""
        BUILD_ALWAYS 1
    )
    ```

4. 编译项目时，出现 `boost_1_69_0.tar.bz2` 下载慢或者下载失败的情况。

    **解决方法**：先停止当前 `make` 流程，然后手动在 [**官网**](https://boostorg.jfrog.io/artifactory/main/release/1.69.0/source/) 下载 `boost_1_69_0.tar.bz2` ，放到项目 `build/thirdparty/Download/boost/` 目录下，然后继续执行 `make` 。

5. 编译项目时，出现 `file cannot create directory` 或 `cannot copy file` 报错的情况，如：

    ``` console
    Install the project...
    -- Install configuration: ""
    CMake Error at cmake_install.cmake:44 (file):
        file cannot create directory:
        /usr/local/venachain.cdt/lib/cmake/venachain.cdt.  Maybe need
        administrative privileges.
    ```

    **原因与解决方法**：用户权限不足，请尝试使用更高权限，比如：

    ``` bash
    su root
    make install
    ```
