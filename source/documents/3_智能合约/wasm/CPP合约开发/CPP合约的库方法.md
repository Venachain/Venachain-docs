# CPP合约的库方法

|    时间    | 修改人 | 修改事项 | 版本  |
| :--------: | :----: | :------: | :---: |
| 2021.07.15 |  杨毅  |   初稿   | 0.0.1 |

## dp

dp 包主要是提供将合约数据存储到链中所需的数据结构的，这里封装了 array、list、map 三种不同的数据结构，以方便对合约数据的操作。

### array.hpp

#### Array简述

array 提供了类似于常规数组的合约数据操作

#### Array构造

```cpp
typedef bcwasm::db::Array<arg1,arg2,arg3> testArray_t;
testArray_t testArray
```

- arg1：所构造Array的名字，在同一个合约中，arg1相同的Array所使用的是同一片空间
- arg2：存入Array的成员的类型
- arg3：Array的长度

#### Array方法

- at 获取指定位置的Array数据

  ```cpp
  Key& at(size_t pos)
  ```

  使用方式：

  ```cpp
  testArray.at(pos)
  ```

  ```{note}
  pos < 0 或 pos > size（Array的长度） 会报越界错误
  ```

- []  与数组一样，支持通过[]操作符来取值

  ```cpp
  Key& operator[](size_t pos)
  ```

  使用方式：

  ```cpp
  testArray[pos]
  ```

  ```{note}
  内部调用的是 testArray.at(pos)，所以越界一样会报错
  ```

- size 获取当前 Array 的长度

  ```cpp
  size_t size() const
  ```

  使用方式：

  ```cpp
  testArray.size()
  ```

- getConst 以常量值的方式获取一个成员，不刷新缓存

  ```cpp
  Key getConst(size_t pos) const
  ```

  使用方式：

  ```cpp
  testArray.getConst(pos)
  ```

- setConst 设置一个常量值到指定位置，不刷新缓存

  ```cpp
  void setConst(size_t pos, const Key &key)
  ```

  使用方式：

  ```cpp
  testArray.setConst(pos, key)
  ```

  ```{note}
  如果 pos < 0 或 pos > size （Array 的长度）会报越界错误
  ```

(cpp_lib_array_iterator)=
#### 迭代器

- 普通迭代器 Iterator

  一般用于合约数据的**写入**操作。

```cpp
/**
 * @brief Construct a new Iterator object
 * 
 * @param array Array
 * @param pos position
 */
Iterator(Array<Name, Key, Size> *array, size_t pos)
        :array_(array), pos_(pos){
}
```

- 反向迭代器 ReverseIterator

  其实就是对普通迭代器重命名，把普通迭代器的 end()  当做  反向迭代器的 begin()，把普通迭代器的 begin() 当做反向迭代器的 end()

- 常量迭代器 ConstIterator

  本质上和 Iterator 是一样的，只是一般用于合约数据的**读取**操作。

  ```cpp
  /**
   * @brief Construct a new Const Iterator object
   * 
   * @param array Array
   * @param pos position
   */
  ConstIterator(Array<Name, Key, Size> *array, size_t pos)
          :array_(array), pos_(pos) {
  }
  ```

- 常量反向迭代器 ConstReverseIterator

  操作性质同 ConstIterator，一般用于**读取**操作，只是迭代顺序与 Iterator 相反。

  ```cpp
  /**
   * @brief Construct a new Const Reverse Iterator object
   * 
   * @param array Array
   * @param pos position
   */
  ConstReverseIterator(Array<Name, Key, Size> *array, size_t pos)
          :array_(array), pos_(pos){
  }
  ```

(cpp_lib_array_operator)=
#### 操作符重载

- ==

  ```cpp
  // == 操作符用于比较两个迭代器是否相等
  friend bool operator == ( const Iterator& a, const Iterator& b ) {
      return a.array_ == b.array_ && a.pos_ = b.pos_;
  }
  ```

- !=

  ```cpp
  // != 操作符用于比较两个迭代器是否不相等
  friend bool operator != ( const Iterator& a, const Iterator& b ) {
      return a.array_ != b.array_ || a.pos_ != b.pos_;
  }
  ```

- \*

  ```cpp
  // * 和 -> 操作符都是用于获取数组中第 pos 个成员的值
  Key& operator*() const {
      return array_->at(pos_-1);
  }
  ```

- ->

  ```cpp
  // * 和 -> 操作符都是用于获取数组中第 pos 个成员的值
  Key& operator->() const {
      return array_->at(pos_-1);
  }
  ```

- \- \- (中间无空格)

  ```cpp
  // -- 操作符使当前迭代器的 pos -1
  Iterator& operator--(){
      pos_--;
      return *this;
  }
  
  // 使用 -- 操作符时，如果 pos < 0 会报错
  Iterator operator --(int) {
      BCWasmAssert(pos_ > 0, "pos can't be negative");
      Iterator tmp(array_, pos_--);
      --tmp;
      return tmp;
  }
  ```

- ++

  ```cpp
  // ++ 操作符是用于使当前迭代器的 pos +1
  Iterator& operator ++() {
      pos_++;
      return *this;
  }
  ```

### list.hpp

#### List简述

list 提供了类似于常规列表的合约数据操作

#### List构造

```cpp
typedef bcwasm::db::List<arg1,arg2> testList_t;
testList_t testList
```

- arg1：所构造List的名字，在同一个合约中，arg1相同的List所使用的是同一片空间
- arg2：存入LIst的成员的类型

#### List方法

- push 添加一个成员到 list 中

  ```cpp
  void push(const Key &k)
  ```

  使用方式：

  ```cpp
  testList.push(key)
  ```

- get 从 list 中获取一个指定下标的成员

  ```cpp
  Key& get(size_t index)
  ```

  使用方式：

  ```cpp
  testList.get(index)
  ```

  ```{note}
  如果 index < 0 或 index > size （List 的长度）会报越界错误
  ```

- del 删除 list 中指定下标下的成员

  ```cpp
  void del(size_t index)
  ```

  使用方式：

  ```cpp
  testList.del(index)
  ```

  ```{note}
  如果 index < 0 或 index > size （List 的长度）会报越界错误
  ```

- del 删除 list 中的指定key，如果 key 不存在则不做任何操作

  ```cpp
  void del(const Key &delKey)
  ```

  使用方式：

  ```cpp
  testList.del(key)
  ```

- []  与数组一样，支持通过[]操作符来取值

  ```cpp
  Key& operator[](size_t i) 
  ```

  使用方式：

  ```cpp
  testList[i]
  ```

  ```{note}
  内部调用的是 testList.get(i)，所以越界一样会报错
  ```

- getConst 以常量值的方式获取一个成员，不刷新缓存

  ```cpp
  Key getConst(size_t index) const
  ```

  使用方式：

  ```cpp
  testList.getConst(index)
  ```

  ```{note}
  如果 index < 0 或 index > size （List 的长度）会报越界错误
  ```

- setConst 设置一个常量值到指定位置，不刷新缓存

  ```cpp
  setConst(size_t index , const Key &key)
  ```

  使用方式：

  ```cpp
  testList.setConst(index,key)
  ```

  ```{note}
  如果 index < 0 或 index > size （List 的长度）会报越界错误
  ```

#### 操作符重载

同 array 的 [操作符重载](cpp_lib_array_operator)

#### 迭代器

同 array 的 [迭代器](cpp_lib_array_iterator)

### map.hpp

#### Map简述

Map提供了对用户数据通过key-value对的形式存储于与合约地址相关的某地址的方法

#### Map构造

```cpp
typedef bcwasm::db::Map<arg1,arg2,arg3> testMap_t;
testMap_t testMap
```

- arg1：所构造Map的名字，在同一个合约中，arg1相同的Map所使用的是同一片空间
- arg2：存入的key的类型
- arg3：存入的value的类型

#### Map方法

- insert 插入数据

  ```cpp
  bool insert(const Key &k, const Value &v)
  ```

  使用方法

  ```cpp
  testMap.insert(key,value);
  ```

  ```{note}
  insert只能插入新的key，如果key值已存在，则返回false
  ```
  
- find 查找数据

  ```cpp
  const Value* find(const Key &k)const

  const Value* find(const Key &k)
  ```

  使用方法

  ```cpp
  value = *testMap.find(key);
  ```

  ```{note}
  find返回值为指针，解引用之前需要先判断指针是否为空
  ```

- update 更新数据

  ```cpp
  bool update(const Key &k,const Value &v)
  ```

  使用方法

  ```cpp
  testMap.update(key,value);
  ```

  ```{note}
  如果更新未存在的key，则返回false
  ```

- del 输出数据

  ```cpp
  void del(const Key &k)
  ```

  使用方法

  ```cpp
  testpMap.del(key);
  ```

- size 查看Map大小

  ```cpp
  int size()

  int size() const
  ```

  使用方法

  ```cpp
  testMap.size();
  ```

#### 其他注意事项

1. 在使用迭代器期间如果需要进行插入或者删除操作，需要自行对迭代器递增或递减

    例如：

    ```cpp
    for(auto it=testMap.begin(); it!=testMap.end();it++)
    {
                std::string a="Hello";
                std::string b="world";
                testMap.insert(a,b);
                it--;
    }
    ```

2. 不支持[]操作，如果需要进行修改操作，可以调用update方法

## bcwasm.hpp

写合约的命名空间

```cpp
namespace bcwasm
```

使用方式：

```cpp
bcwasm::sm2secSigVerify()
```

## crypto.hpp

### 简述

对外提供了一些加密方法

### 方法

- smSigVerify  

  ```cpp
  void smSigVerify(const char* _msg, int _msgSize, const char* _userid, int _useridSize, const char* _pubkey, int _pubkeySize, const char * _sig, int _sigSize, char* _result, int _resultSize);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  std::string sigVerify(const std::string& _msg, const std::string& _userid, const std::string& _pubkey, const std::string& _sig)
  ```

  使用方式：

  ```cpp
  bcwasm::sigVerify(msg, userid, pubkey, sig)
  ```

- sm2secSigVerify

  ```cpp
  void sm2secSigVerify(const char* _msg, int _msgSize, const char* _pubkey, int _pubkeySize, const char * _sig, int _sigSize,  char* _result, int _resultSize);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  std::string sm2secSigVerify(const std::string& _msg, const std::string& _pubkey, const std::string& _sig)
  ```

  ​	使用方式：

  ```cpp
  bcwasm::sm2secSigVerify(msg, pubkey, sig)
  ```

- secp256r1SigVerify

  ```cpp
  void secp256r1SigVerify(const char* _msg, int _msgSize, const char* _pubkey, int _pubkeySize, const char * _sig, int _sigSize, char* _result, int _resultSize);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  std::string secp256r1SigVerify(const std::string& _msg, const std::string& _pubkey, const std::string& _sig)
  ```

  使用方式：

  ```cpp
  bcwasm::secp256r1SigVerify(msg, pubkey, sig)
  ```

- secp256k1SigVerify

  ```cpp
  void secp256k1SigVerify(const char* _msg, int _msgSize, const char* _pubkey, int _pubkeySize, const char * _sig, int _sigSize, char* _result, int _resultSize);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  std::string secp256k1SigVerify(const std::string& _msg, const std::string& _pubkey, const std::string& _sig)
  ```

  使用方式：

  ```cpp
  bcwasm::secp256k1SigVerify(msg, pubkey, sig)
  ```

- sm3

  ```cpp
  void sm3(const char* _msg, int _msgSize, char* _result, int _resultSize);  
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  std::string sm3Compute(const std::string& _msg)
  ```

  使用方式：

  ```cpp
  bcwasm::sm3Compute(msg)
  ```

```{note}
上述方法只是以接口的形式存在于 C++ 代码中，具体实现是在 Venachain 项目中以 Golang 代码来实现的，运行的时候会自动匹配调用其具体实现。
```

## RLP.h

### 简述

一种区块链的递归编码规则，可用于编码任意嵌套的二进制数。

其序列化与反序列化方法被封装在 bcwasm/serialize.hpp 中，推荐以 bcmasm 的方式对其进行使用，下面的内容也是以 bcwasm 封装的方式来写的，内容在 bcwasm/serialize.hpp 中。

### 序列化

- 定义

```cpp
#define BCWASM_SERIALIZE( TYPE,  MEMBERS )
```

TYPE：需要进行序列化与反序列化的类

MEMBERS：该类需要序列化的一系列成员名称，传入方式为：(field1)(field2)(field3)

- 使用方法

```cpp
// 学生类型属性
struct SudentType
{
    string name; // 名称
    int age;  // 年龄
    int gender; // 性别
    // RLP 序列化
    BCWASM_SERIALIZE(SudentType, (name)(age)(gender));
};
```

### 反序列化

- 定义

  ```cpp
  #define BCWASM_SERIALIZE_DERIVED( TYPE, BASE, MEMBERS )
  ```

  TYPE：需要进行序列化与反序列化的类

  BASE：被序列化的类的成员名称，传入方式为：(field1)(field2)(field3)

  MEMBERS：反序列化的类的成员名称，传入方式为：(field1)(field2)(field3)

- 使用方法

  --- TODO

## deployedcontract.hpp

### 简述

deployedcontract.hpp 提供了一些合约间调用的相关方法

### 方法

- bcwasmCallString

  调用其他合约的函数，返回的数据类型是 string

  ```cpp
  char* bcwasmCallString(const uint8_t *address, const uint8_t *args, uint32_t len);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  inline std::string callString(const std::string &funcName, Args&&... args) const
  ```

  使用方式：

  ```cpp
  bcwasm::callString(funcName, arg1, arg2, arg3)
  ```

- bcwasmCallInt64

  调用其他合约的函数，返回的数据类型是 int64

  ```cpp
  int64_t bcwasmCallInt64(const uint8_t *address, const uint8_t *args, uint32_t len);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  inline int64_t callInt64(const std::string &funcName, Args&&... args) const
  ```

  使用方式：

  ```cpp
  bcwasm::callInt64(funcName, arg1, arg2, arg3)
  ```

- bcwasmDelegateCallString

  代理调用其他合约的函数，返回的数据类型是 string

  ```cpp
  char* bcwasmDelegateCallString(const uint8_t *address, const uint8_t *args, uint32_t len);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  inline std::string delegateCallString(const std::string &funcName, Args&&... args) const
  ```

  使用方式：

  ```cpp
  bcwasm::delegateCallString(funcName, arg1, arg2, arg3)
  ```

- bcwasmDelegateCallInt64

  代理调用其他合约的函数，返回的数据类型是 int64

  ```cpp
  int64_t bcwasmDelegateCallInt64(const uint8_t *address, const uint8_t *args, uint32_t len);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  inline int64_t delegateCallInt64(const std::string &funcName, Args&&... args) const
  ```

  使用方式：

  ```cpp
  bcwasm::delegateCallInt64(funcName, arg1, arg2, arg3)
  ```

- bcwasmCall

  调用其他合约的函数，无返回值

  ```cpp
  void bcwasmCall(const uint8_t *address, const uint8_t *args, uint32_t len);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  inline void call(const std::string &funcName, Args&&... args) const
  ```

  使用方式：

  ```cpp
  bcwasm::call(funcName, arg1, arg2, arg3)
  ```

- bcwasmDelegateCall

  代理调用其他合约的函数，无返回值

  ```cpp
  void bcwasmDelegateCall(const uint8_t *address, const uint8_t *args, uint32_t len);
  ```

  bcwasm 命名空间该方法进行了封装，推荐以 bcwasm 命名空间的方式对该方法进行使用。

  ```cpp
  inline void delegateCall(const std::string &funcName, Args&&... args) const
  ```

  使用方式：

  ```cpp
  bcwasm::delegateCall(funcName, arg1, arg2, arg3)
  ```

## event.hpp

### 简述

对外提供了事件的定义和触发方法

### 定义事件

- BCWASM_EVENT

  BCWASM_EVENT 是用来定义事件的，参数包含事件名称和事件所需的其他参数

  ```cpp
  #define BCWASM_EVENT(NAME, ...) \
      void M_CAT(EVENT, NAME)(VA_F(__VA_ARGS__)) { \
          bcwasm::emitEvent(#NAME, PA_F(__VA_ARGS__)); \
      }
  ```

  NAME：事件名称

  该事件名称必须是在写合约时，在最后定义的函数所生成的ABI文件里面定义过的事件才能支持。

  使用方式：

  ```cpp
  // 定义Event.
  BCWASM_EVENT(Notify, uint64_t, const char *)
  ```

### 触发事件

- BCWASM_EMIT_EVENT

  BCWASM_EMIT_EVENT 是用来触发事件的，参数包含事件名称和事件所需的其他参数

  ```cpp
  #define BCWASM_EMIT_EVENT(NAME, ...) \
      M_CAT(EVENT, NAME)(__VA_ARGS__)
  ```

  NAME：事件名称

  该事件名称必须是在写合约时，在最后定义的函数所生成的ABI文件里面定义过的事件才能支持。

  使用方式：

  ```cpp
  // 触发事件
  BCWASM_EMIT_EVENT(Notify, code, (string(json) + string("is not a json object")).c_str());
  ```

## exception.h

由 bcwasm 封装好的抛异常方法，可以用来手动抛出异常。

```cpp
template<typename Arg, typename... Args>
void bcwasmThrow(Arg&& a, Args&&... args){
    println(a, args...);
    abort();
}
```

使用方式：

```cpp
bcwasm::bcwasmThrow(a,b,c)
```

## nizkpail.hpp

### 简述

提供了零知识证明相关的方法。

### 方法

```cpp
void pailEncrypt(const char* _number, int _numberSize, 
                    const char* _pubkey, int _pubkeySize,
                    char* _result, int _resultSize);

void pailHomAdd(const char* _cipher1, int _cipher1Size,
                    const char* _cipher2, int _cipher2Size,
                    const char* _pubkey, int _pubkeySize,
                    char* _result, int _resultSize);

void pailHomSub(const char* _cipher1, int _cipher1Size,
                    const char* _cipher2, int _cipher2Size,
                    const char* _pubkey, int _pubkeySize,
                    char* _result, int _resultSize);

void nizkVerifyProof(const char* _pai, int _paiSize,
                    const char* _fromBalCipher, int _fromBalCipherSize,
                    const char* _fromAmountCipher, int _fromAmountCipherSize,
                    const char* _toAmountCipher, int _toAmountCipherSize,
                    const char* _fromPubkey, int _fromPubkeySize,
                    const char* _toPubkey, int _toPubkeySize,
                    char* _result, int _resultSize);
```

上述方法 bcwasm 命名空间都对它们做了封装，推荐以 bcwasm 命名空间的方式对它们进行使用。下面是 bcwasm 对它们的封装：

```cpp
std::string pailEncrypt(const std::string& _number, const std::string& _pubkey)
    
std::string pailHomAdd(const std::string& _cipher1, const std::string& _cipher2, const std::string& _pubkey)
    
std::string pailHomSub(const std::string& _cipher1, const std::string& _cipher2, const std::string& _pubkey)
    
std::string nizkVerifyProof(const std::string& _pai,
								const std::string& _fromBalCipher,
								const std::string& _fromAmountCipher,
								const std::string& _toAmountCipher,
								const std::string& _fromPubkey,
								const std::string& _toPubkey)
```

使用方式如：

```cpp
bcwasm::pailEncrypt(number, pubkey)
```

## print.hpp

提供了一些用于打印输入的方法。

```cpp
void prints( const char* cstr );

void prints_l( const char* cstr, uint32_t len);

void printi( int64_t value );

void printui( uint64_t value );

void printi128( const int128_t* value );

void printui128( const uint128_t* value );

void printsf(float value);

void printdf(double value);

void printqf(const long double* value);

void printhex( const void* data, uint32_t datalen );
```

使用方式如：

```cpp
bcwasm::prints("Hello World!"); // Output: Hello World!
```

## serialize.hpp

是对 RLP 的封装，提供对数据进行 RLP 序列化与反序列化的方法，详情请看前面 RLP.h 的说明。

## state.hpp

### 简述

提供了与链相关的方法

### 方法

- blockHash

  获取区块的哈希值

  ```cpp
  /**
   * @brief Get block hash
   * @param number Block height
   * @return h256 Block hash
   */
  h256 blockHash(int64_t number)
  ```

  使用方式：

  ```cpp
  bcwasm::blockHash(number)
  ```

- coinbase

  获取当前区块矿工的地址

  ```cpp
  /**
   * @brief Current block miner’s address
   * @return h160 
   */
  h160 coinbase()
  ```

  使用方式：

  ```cpp
  bcwasm::coinbase()
  ```

- balance

  获取链上账号的余额

  ```cpp
  /**
   * @brief Get the balance
   * @return u256 balance
   */
  u256 balance() 
  ```

  使用方式：

  ```cpp
  bcwasm::balance()
  ```

- origin

  获取事物的发送方

  ```cpp
  /**
   * @brief Sender of the transaction (full call chain)
   * @return h160 
   */
  h160 origin()
  ```

  使用方式：

  ```cpp
  bcwasm::origin()
  ```

- caller

  获取调用者地址

  ```cpp
  /**
   * @brief Caller address
   * @return h160
   */
  h160 caller()
  ```

  使用方式：

  ```
  bcwasm::caller()
  ```

- isOwner

  判断帐户是否为合同的创建者

  ```cpp
  /**
   * @brief whether account is contract's creator
   * @return int64_t
   */
  int64_t isOwner(const Address &contract, const Address &account)
  ```

  使用方式：

  ```cpp
  bcwasm::isOwner(contract, account)
  ```

- isFromInit

  判断某些方法是否是来自初始化调用

  ```cpp
  /**
   * @brief Is from init
   * @return h160
   */
  int64_t isFromInit()
  ```

  使用方式：

  ```cpp
  bcwasm::isFromInit()
  ```

- callValue

  获取随消息发送的消息数量

  ```cpp
  /**
       * @brief Number of wei sent with the message
       * @return u256 
       */
      u256 callValue()
  ```

  使用方式：

  ```
  bcwasm::callValue()
  ```

- address

  获取合约地址

  ```cpp
  /**
       * @brief Contract address
       * @return h160 
       */
      h160 address()
  ```

  使用方式：

  ```cpp
  bcwasm::address()
  ```

- sha3

  计算输入数据的Keccak-256哈希

  ```cpp
  /**
       * @brief Compute the Keccak-256 hash of the input
       * @param data String
       * @return h256 
       */
      h256 sha3(const std::string &data) 
          
  /**
       * @brief Compute the Keccak-256 hash of the input
       * @param data Byte pointer
       * @param len Length
       * @return h256 
       */
      h256 sha3(const byte *data, size_t len)
  ```

  使用方式：

  ```cpp
  bcwasm::sha3(data)
  bcwasm::sha3(data, len)
  ```

- callTransfer

  发送指定数量的 Wei 到指定地址

  ```cpp
  /**
       * @brief Send given amount of Wei to Address
       * 
       * @param to Destination address
       * @param amount Amount
       * @return int64_t 0 success non-zero failure
       */
      int64_t callTransfer(const Address& to, u256 amount)
  ```

  使用方式：

  ```cpp
  bcwasm::callTransfer(to, amount)
  ```

- ecrecover

  回收

  ```cpp
  /**
   * @brief ecrecover
   * @param h, hash of the message
   * @sig, signature of the message
   * @return h160 
   */
  h160 ecrecover(const byte* h, const byte* sig)
  
  h160 ecrecover(bytes & h, bytes & sig)
  ```

  使用方式：

  ```cpp
  bcwasm::ecrecover(h, sig)
  ```

## storage.hpp

### 简述

提供数据存储相关的操作方法。

前面的 array、list、map 其实底层都是调用了 storage.hpp 的相关方法的，也可以不使用array、list、map而直接调用  storage.hpp 的方法来直接存储数据。

### 方法

- setState

  设置状态对象

  ```cpp
  /**
   * @brief Set the State object
   * 
   * @tparam KEY Key type
   * @tparam VALUE Value type
   * @param key Key
   * @param value Value
   */
  template <typename KEY, typename VALUE>
  inline void setState(const KEY &key, const VALUE &value) 
  ```

  使用方式：

  ```cpp
  bcwasm::setState(key, value)
  ```

- getState

  获取状态对象

  ```cpp
  /**
   * @brief Get the State object
   * 
   * @tparam KEY Key type
   * @tparam VALUE Value type
   * @param key Key
   * @param value Value
   * @return size_t Get the length of the data
   */
  template <typename KEY, typename VALUE>
  inline size_t getState(const KEY &key, VALUE &value) 
  ```

  使用方式：

  ```cpp
  bcwasm::getState(key, value)
  ```

- delState

  删除状态对象

  ```cpp
  /**
   * @brief delete State Object
   * 
   * @tparam KEY Key type
   * @param key Key
   */
  template <typename KEY>
  inline void delState(const KEY &key)
  ```

  使用方式：

  ```cpp
  bcwasm::delState(key)
  ```

  

