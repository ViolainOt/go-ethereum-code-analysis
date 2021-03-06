
### 1.geth内部代码结构

* abigen：一个源代码生成器，它将Ethereum智能合约定义(代码) 转换为易于使用的、编译时类型安全的Go package。 如果合约字节码也available的话，它可以在普通的Ethereum智能合约ABI上扩展功能。 同时也能编译Solidity源文件，使开发更加精简。

* bootnode：该节点为Ethereum发现协议运行一个引导节点。

* clef：Clef可以用来签署交易和数据，并且可以代替geth的账户管理。这使DApps不依赖于geth的账户管理。 当DApp想要签署数据时，它可以发送数据签名者，签名者将向用户提供上下文，并要求用户签署数据的许可。 如果用户授予签名者将签名发回给DApp的签名请求。此设置允许DApp连接到远程以太坊节点并发送本地签名的事务。 这个可以当DApp连接到远程节点时会有所帮助，因为本地Ethereum节点不可用，而不是与没有内置（或有限）帐户管理的链或特定以太坊节点同步。Clef可以在同一台机器上作为守护进程运行，或者关闭一个usb-stick，如[usbarmory]（https://inversepath.com/usbarmory），或[QubesOS]（https://www.qubes-os.org/）类型os设置中的单独虚拟机。

* ethkey

* evm：执行EVM代码片段

* faucet：faucet是以太faucet支持的轻量级客户

* geth：geth是Ethereum的官方客户端命令行，它是Ethereum网络（以太坊主网，测试网络或私有网）的入口点，使用此命令可以使节点作为full node（默认），或者archive node（保留所有历史状态）或light node（检索数据实时）运行。 其他进程可以通过暴露在HTTP，WebSocket和/或IPC传输之上的JSON RPC端点作为通向Ethereum网络的网关使用

* internal   

* p2psim：p2psim为客户端命令行模拟HTTPAPI

* puppeth：puppeth是一个命令组装和维护私人网路

* rlpdump：rlpdump能更好的打印出RLP格式的数据 

* swarm：bzzhash命令能够更好的计算出swarm哈希树

* utils: 为Go-Ethereum命令提供说明

* wnode   

### 2.Eth相关代码结构

#### 一级目录

* eth：以太坊协议

* ethclient ：以太坊RPC API客户端 

#### 2.二级目录

* downloader：手动全链同步，主要负责区块链最开始的同步工作，当前的同步有两种模式， 一种是传统的fullmode,这种模式通过下载区块头，和区块体来构建区块链，同步的过程就和普通的区块插入的过程一样，包括区块头的验证，交易的验证，交易执行，账户状态的改变等操作，这其实是一个比较消耗CPU和磁盘的一个过程。另一种模式就是 快速同步的fast sync模式， 这种模式有专门的文档来描述。请参考fast sync的文档。简单的说 fast sync的模式会下载区块头，区块体和收据，插入的过程不会执行交易，然后在一个区块高度(最高的区块高度 - 1024)的时候同步所有的账户状态，后面的1024个区块会采用fullmode的方式来构建。 这种模式会加区块的插入时间，同时不会产生大量的历史的账户信息。会相对节约磁盘， 但是对于网络的消耗会更高。因为需要下载收据和状态。

* fetcher：基于块通知的同步。接收到当我们接收到NewBlockHashesMsg消息得时候，我们只收到了很多Block的hash值。 需要通过hash值来同步区块

* filters：用于区块，交易和日志事件的过滤，包包含了给用户提供过滤的功能，用户可以通过调用对交易或者区块进行过滤，然后持续的获取结果，如果5分钟没有操作，这个过滤器会被删除。

* gasprice：提供gas的价格建议， 根据过去几个区块的gasprice，来得到当前的gasprice的建议价格

* tracers：收集JavaScript交易追踪 
