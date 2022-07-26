# VenaAgent 安装

## 环境准备

### 硬件环境准备

- 主  机：Intel Xeon E5-2650或以上。

- 内  存：16G或以上。

- 硬  盘：32GB或以上。

- 图形卡：VGA/DVI。

### 软件环境准备

- OS	Linux 64bit
- Golang 1.14.4 或以上
- Venachain v1.0.0 或以上
- MongoDB 4.2.8 或以上

## 安装步骤

1. 下载、安装和配置 Golang

2. 下载、安装和启动 MongoDB
   
3. 下载和编译 Venachain

   - 下载地址：[venachain可执行文件下载](https://git-c.i.wxblockchain.com/vena/src/venachain/-/tags)

   - 部署

      请参考 [Venachain部署文档](../../../2_区块链部署/Venachain部署指南.md) 进行链的部署

4. 下载 VenaAgent 特定版本 X.X.X

   - 下载 VenaAgent 后端源码： [venaagent 可执行文件下载](https://git-c.i.wxblockchain.com/vena/src/venaagent/-/tags)

   - 重命名：

      ```sh
      mv venaagent-X.X.X.zip ./venaagent.zip
      
      ```

5. 配置 VenaAgent 后端

   5.1. 进到 venaagent 目录下。

   5.2. 在 `config.toml` 文件中配置 venaagent 基础配置
      
      - 5.2.1 通用配置

      ```toml
      [common]
      # 代理服务器私钥文件
      key_file_path = "./conf/keyfile.json"
      # 代理服务器私钥密码
      passphrase = "0"
      # 合约存储地址
      contract_dir = "./data/contract"
      # 数据库类型, 1. sqlite; 2. mongo
      db_type = "sqlite"
      ```

     - 5.2.2 配置 ip 及 端口号

      ```toml
      [http]
      # 运行的 ip 地址
      ip = "127.0.0.1"
      # 运行的 端口号
      port = "9999"
      # mode 必须是 "release"、"debug"、"test" 中的一个
      mode = "test"
      # cors 跨域资源共享白名单(非前端调用,不必填)
      cors = ""

      [rpc]
      # 运行的 rpc ip 地址
      ip = "127.0.0.1"
      # 运行的 rpc port
      port = "19999"
      ```

     - 5.2.3 配置 https 证书，启用 https 部署，enable 需要设置为 true

      ```toml
      [tls]
      # 是否使用 https 部署
      enable = false
      # 证书路径
      cert_file = "./conf/cert.pem"
      # 私钥文件路径
      key_file = "./conf/key.pem"
      ```

   5.3. 在 `config.toml` 文件中配置 venaagent 所需数据库信息
     - 5.3.1 配置 mongo 信息 **( 因为使用了事务操作, mongo 需要搭建副本集模式 )**
 
      ```toml
      [mongo]
      # 数据库地址
      ip = "127.0.0.1"
      # 数据库端口
      port = "27017"
      # 数据库用户名
      username = "root"
      # 数据库密码
      password = "root"
      # 数据库库名
      dbname = "vena_agent"
      # 数据库连接超时时长，单位毫秒
      connTimeout = 10000
      # 数据库查询超时时长，单位毫秒
      selectTimeout = 10000
      ```
      
      - 5.3.2 配置 sqlite 信息

      ```toml
      [sqlite]
      data_dir = "./data/sqlite"
      dbname = "vena.db"
      # 慢查询阈值,单位毫秒
      slow_threshold = 800
      # 连接最大生命周期,单位秒
      conn_max_life_time = 3600
      # 最大连接数
      max_open_conns = 5
      ```

   5.4. 在 `config.toml` 文件中配置 venaagent 服务基本配置信息
    
   - 配置系统默认配置信息

      ```toml
      [system_config]
      # 最大等待发送交易时长, 单位为 毫秒
      max_sync_send_tx_duration = 5000
      # 节点连接超时时长, 单位为 毫秒
      node_conn_timeout = 3000
      # 发送交易重试次数
      net_fail_retry_count = 3
      ```

   - 配置链池管理信息

      ```toml
      [chain_pool]
      # 节点最大可延迟的区块高度（超过则视为不可用节点）
      max_delay_block = 1
      # 最大空闲连接数
      max_idle_conns = 5
      # 最大空闲连接超时，单位毫秒
      idle_conn_timeout = 120000
      # 失败等待下论重连时间组，逐步递增，单位毫秒
      try_conn_wait_times = [2000,6000,15000,120000,600000]
      # 检查节点可用性周期，单位毫秒
      node_usable_check_period = 30000
      # 尝试获取客户端的间隔，单位毫秒
      try_get_client_interval = 2000
      # 尝试获取客户端次数
      try_get_client_frequency = 3
      ```
   
   - 配置监听模块

      ```toml
      [notification]
      # 发送订阅消息通道堆积容量
      send_message_chan_size = 2000
      # 发送订阅消息的协程数
      send_message_work_num = 1
      ```

   - 配置订阅模块

      ```toml
      [subscriber]
      # 监听到的交易回执批量入库条目数
      sub_tx_receipt_batch_insert_num = 5000
      ```

   - 私钥托管服务配置 ( 该功能仅用来提供交易发起人私钥，对发起的交易进行签名，并不保证托管的私钥不会丢失，使用时仍需要对用户私钥进行额外备份 )

      ```toml
      [key_trust]
      # keyfile 默认口令
      password = "0"
      # 私钥解锁默认周期, 单位 秒
      duration = 300
      # 保存私钥文件目录地址
      keys_file_dir = "./data/keys"
      ```

    5.5. 配置 venaagent 链信息

    - 配置 keyfile.json
    
      a. 可以根据私钥产生 keyfile.json 文件或者在链上创建新用户，并获取 keyfile 内容信息，将其存放至 keyfile.json 文件；

      b. 将拿到的 keyfile.json 文件存放至 conf 目录 (可配置，见 5.2.1 【通用配置】) 下，作为链代理服务器本身私钥，用于发起通用交易及调用合约时，在未指定发起方时对交易进行加密
       
    - 在 `config.toml` 文件中，配置代理链信息

      ```toml
      [chain]
      # 链标识名 (唯一)
      name = "vena"
      # 链节点网络地址，多节点使用逗号分隔
      endpoints = "127.0.0.1:6791,127.0.0.1:6792"
      # 订阅消息缓冲区大小
      subscriber_buff_size = 1024
      # websocket 检查服务状态间隔（网络问题等，需要重订阅），单位毫秒
      check_server_status_interval = 500
      # 链节点 websocket 端口号配置，key 为 节点标识value 为端口号
      [chain.nodeWSPort]
      0 = 26791
      1 = 26792
      2 = 26793
      3 = 26794
      ```

6. 启动 VenaAgent

     进到 venaagent 目录下，执行以下命令

     ```sh
     go build -mod=mod -o venaagent
     nohup ./venaagent > ./venaagent.log 2>&1 &
     ```

7. 通过 /swagger/index.html 可查看相关接口定义或查看【接口文档】，同时支持 grpc 调用，相关传输 proto 文件存放在 ./rpc/message 目录下，根据不同开发语言，进行编译调用即可

8. rpc 提供了监听交易回执功能，可以通过以下方式进行监听

   ```go
   func TestNotification(t *testing.T) {
      clientConn, err := util.GetRPCClientConn()
      if err != nil {
         fmt.Println(err)
         return
      }
      defer clientConn.Close()
      cli := message.NewNotifServiceClient(clientConn)

      stream, err := cli.Subscribe(context.Background(), &message.SubscribeReq{
         ChainNames: []string{"vena"},
         ClientId:   "aaa2",
         TopicTypes: []message.TopicType{message.TopicType_TXRECEIPT},
      })
      defer stream.CloseSend()
      if err != nil {
         log.Fatalf("failed to subscribe %v", err)
      }
      for {
         resp, err := stream.Recv()
         if err != nil {
            fmt.Println(err)
            return
         }
         if err == io.EOF {
            fmt.Println("stop")
            return
         }
         fmt.Println(resp)
      }
   }
   ```

9. 本版本同时支持 websocket 通信
- 可以通过 websocket 订阅核心目标链相关主题事件
- 支持取消事件订阅
- 支持使用 websocket 完成所有功能请求（发送交易、查询等）

   ```go
   var addr = flag.String("addr", "127.0.0.1:9999", "http service address")

   func TestClient(t *testing.T) {
      u := url.URL{Scheme: "ws", Host: *addr, Path: ""}
      dialer := websocket.Dialer{TLSClientConfig: &tls.Config{RootCAs: nil, InsecureSkipVerify: true}}
      c, _, err := dialer.Dial(u.String(), nil)
      if err != nil {
         fmt.Println(err)
         return
      }
      defer c.Close()
      isClose := false
      go func() {
         for {
            _, message, err := c.ReadMessage()
            if err != nil {
               fmt.Println("read:", err)
               isClose = true
               return
            }
            fmt.Printf("recv: %s", message)
            fmt.Println()
         }
      }()
      go func() {
         tickerChan := time.NewTicker(5 * time.Second).C
         for range tickerChan {
            c.WriteMessage(websocket.PingMessage, []byte(""))
         }
      }()
      scanner := bufio.NewScanner(os.Stdin)
      for scanner.Scan() {
         if isClose {
            break
         }
         scannerText := scanner.Text()
         if scannerText == "1" {
            uploadContractAbiFile(c)
         } else {
            c.WriteMessage(websocket.TextMessage, []byte(scanner.Text()))
         }
      }
   }

   func uploadContractAbiFile(c *websocket.Conn) {
      filePath := "./temp.abi.json"
      file, err := os.OpenFile(filePath, os.O_RDONLY, 0644)
      if err != nil {
         return
      }
      fileBytes, err := ioutil.ReadAll(file)
      if err != nil {
         return
      }
      uploadAbiFileReq := &uploadAbiFileReq{
         Type:    2,
         File:    fileBytes,
         Address: "0xb306ca6cd684dcff93fd81315e0d298b0d3f9d79",
         Desc:    "dd",
      }
      uploadAbiFileReqBytes, _ := json.Marshal(uploadAbiFileReq)
      wsReqMsg := &WsReqMsg{
         ReqType: 11,
         Msg:     string(uploadAbiFileReqBytes),
         ReqId:   "contract" + uuid.New().String(),
      }
      wsMsgReqByte, _ := json.Marshal(wsReqMsg)
      c.WriteMessage(websocket.TextMessage, wsMsgReqByte)
   }

   /*
      type ReqType int32

      const (
         SubTopicReqType ReqType = iota + 1
         CancleSubReqType
         GetBlockByHashReqType
         GetBlockByNumberReqType
         GetTxReceiptReqType
         GetTxByHashReqType
         SendTxReqType
         SendTxAsyncReqType
         SendSigTxReqType
         SendSigTxAsyncReqType
         UploadAbiFileReqType
         ExecuteReqType
         ExecuteAsyncReqType
         GetContractsReqType
         GetChainReqType
         GetChainsReqType
         SetChainReqType
         DeleteChainReqType
         SetSystemConfigReqType
         GetSystemConfigReqType
         GetAccountListReqType
         UploadKeyFileReqType
         SubmitKeyReqType
         UnlockKeyReqType
      )

   具体调用可参考 ./ws/client_test.go 文件
   */
   ```

**注意事项：**
1. 服务初始时，会根据配置的链信息加载链，如果配置的链异常，服务启动将失败

2. 服务初始时，会根据系统配置来设定系统运行参数，后续可通过【系统管理】来修改系统运行参数

3. 若使用 mongo 作为数据持久化方案，需要部署 **副本集模式**，否则在节点信息同步、账户解锁操作中会出现异常

4. 私钥托管功能仅用来提供交易发起人私钥，对发起的交易进行签名，并不保证托管的私钥不会丢失，使用时仍需要对用户私钥进行额外备份

5. 若使用到了私钥托管模块，建议使用 https 进行项目部署（【tls】中 enable 设置为 true，并配置所需的证书及证书密钥文件路径），以保证私钥上传的安全性
