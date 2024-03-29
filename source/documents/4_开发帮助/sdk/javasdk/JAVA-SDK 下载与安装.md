# JAVA-SDK 下载与安装

## SDK 下载

请首先下载SDK最新版本的发布包，[**下载地址**](https://github.com/Venachain/client-sdk-java)。

将发布包解压到本地目录，如下所示：

```shell
# 下载
https://github.com/Venachain/client-sdk-java/releases/tag/v1.0.1
# 解压
tar -zxvf venachain-sdk-java-v1.0.0.0.0.tar.gz
```

## SDK 安装

安装依赖环境

- Java版本：Java 8+ (jdk 1.8+)
- Gradle版本：4.3+

SDK默认使用gradle作为管理工具，若使用maven需要在pom.xml中添加如下的依赖项：

```xml
<dependencies>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.5</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.5</version>
    </dependency>
    <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>3.8.1</version>
    </dependency>
    <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>logging-interceptor</artifactId>
        <version>3.8.1</version>
    </dependency>
    <dependency>
        <groupId>io.reactivex</groupId>
        <artifactId>rxjava</artifactId>
        <version>1.2.4</version>
    </dependency>
    <dependency>
        <groupId>org.java-websocket</groupId>
        <artifactId>Java-WebSocket</artifactId>
        <version>1.3.8</version>
    </dependency>
    <dependency>
        <groupId>com.github.jnr</groupId>
        <artifactId>jnr-unixsocket</artifactId>
        <version>0.15</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.8.5</version>
    </dependency>
    <dependency>
        <groupId>org.bouncycastle</groupId>
        <artifactId>bcprov-jdk15on</artifactId>
        <version>1.54</version>
    </dependency>
</dependencies>
```

将解压目录下的jar包添加至项目中即可。