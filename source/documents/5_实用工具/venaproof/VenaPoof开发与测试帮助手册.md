# VenaPoof 开发与测试帮助手册

| **时间**   | **修改人** | **修改事项** | **存证平台版本** | **文档版本** |
| ---------- | ---------- | ------------ | ---------------- | ------------ |
| 2022.05.20 | 吴经文     | 初稿         | 0.0.1            | 1.0          |

## 编译VenaProof

```{note}
如果想要编译 `venaproof` 可执行文件用于开发调试，可以看以下内容
```

### 1. 编译Venachain

1. 获取项目

	```bash
	git clone https://git-c.i.wxblockchain.com/vena/src/venachain.git
	```

2. 编译Venachain

	请参考 [Venachain编译](../../6_深入使用指南/Venachain编译.md)

### 2. 编译VenaProof

1. 获取项目

	```bash
	git clone https://git-c.i.wxblockchain.com/vena/src/venaproof.git
	```

2. 修改go.mod

	将venachain的目录所在位置填入 `${venachain_path}`
	```console
	replace (
		github.com/Venachain/Venachain => ${venachain_path}
		github.com/go-interpreter/wagon v0.6.0 => github.com/perlin-network/wagon v0.3.1-0.20180825141017-f8cb99b55a39
	)
	```

3. 编译

	编译后的二进制文件会生成在 `release/bin/` 目录下

	```console
	cd ${venaproof_path}
	make clean && make
	```

## 前后端联调测试

### 1. 部署server端

根据文档 [VenaProof安装](./VenaProof安装.md) 将VenaProof安装在堡垒机 `10.230.48.18` ，且暴露的端口必须为 `17017` 。VenaProof项目的测试在该堡垒机上进行。

### 2. 部署web端

1. 向项目相关同事获取前端压缩包。
2. 解压压缩包
3. 启动

	```bash
	## 进入项目目录
	cd client-official-website/wxbc-pc

	## 启动项目
	npm run dev
	```

### 3. 访问

1. 输入地址

	访问地址为前端部署的ip地址的8080端口 ：`http://${ip_addr}:8080`

2. 进入存证平台

	上方导航栏点击 `基础设施` -> 选择 `Venachain` --> 点击 `存证平台` 按钮

