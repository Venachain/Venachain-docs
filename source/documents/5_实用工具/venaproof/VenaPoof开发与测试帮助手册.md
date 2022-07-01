# VenaPoof 开发与测试帮助手册

| **时间**   | **修改人** | **修改事项**                                                 | **存证平台版本** | **文档版本** |
| ---------- | ---------- | ------------------------------------------------------------ | ---------------- | ------------ |
| 2022.05.20 | 吴经文     | 初稿                                                         | 0.0.1            | 1.0          |
| 2022.07.01 | 吴经文     | 修改venaproof编译方法<br>修改文档表述错误，服务端部署在任意堡垒机开放端口都可以访问接口 | 0.0.1            | 2.0          |

## 编译VenaProof

```{note}
如果想要编译 `venaproof` 可执行文件用于开发调试，可以看以下内容
```

### 获取VenaProof项目

```bash
git clone https://git-c.i.wxblockchain.com/vena/src/venaproof.git
```

```{warning}
Venachain与Venaproof的项目目录必须在同一个目录下
```

### 编译

编译后的二进制文件会生成在 `release/bin/` 目录下

```
cd ${venaproof_path}
make clean && make
```

## 前后端联调测试

### 部署web端

1. 向项目相关同事获取前端压缩包。

2. 解压压缩包

3. 修改配置

   修改 `client-official-website/wxbc-pc/vue.config.js` 中的proxy，填写venaproof部署的地址

   修改 `client-official-website/wxbc-pc/src/api/base.js`中的base，填写venaproof部署的地址

4. 启动

  ```bash
  ## 进入项目目录
  cd client-official-website/wxbc-pc
  
  ## 启动项目
  npm run dev
  ```

### 访问网页

1. 输入地址

	访问地址为前端部署的ip地址的8080端口 ：`http://${ip_addr}:8080`

2. 进入存证平台

	上方导航栏点击 `基础设施` -> 选择 `Venachain` --> 点击 `存证平台` 按钮

