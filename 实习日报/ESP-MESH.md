#ESP-MESH

## 1. 树型拓扑
![节点类型](https://img-blog.csdnimg.cn/20190923201720582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

* 以上图可知，Ａ根节点(root Node);Ｂ~J是中间父节点(Intermediate Parent Nodes);L~N是叶子节点(Leaf Nodes);K~O是空闲节点(Idle Node)。
[ESP-MESH概念](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)

## 2. 信标帧和RSSI阈值处理
* RSSI阈值是一种ESP-MESH功能，可以简单地过滤掉所有接收到的低于预定阈值的信标帧。
[信标帧和RSSI阈值处理详解](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)

* 节点的选择策略
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190923203122240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
　　首选父节点的选择将始终优先考虑网络最浅层（包括根节点）上的候选父节点。当形成上游连接时，这有助于最小化ESP-MESH网络中的总层数。例如，给定第二层节点和第三层节点，第二层节点将始终是首选的。
　　如果在同一层中有多个父节点候选者，则子节点最少的父节点候选者将是首选。该标准具有平衡同一层节点之间的下游连接数量的效果。

## [3.路由表](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)
* ESP-MESH利用路由表根据以下规则确定是应向上游还是向下游转发ESP-MESH数据包。

## [4.构建网络](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)

* 限制树深度:网络的最大允许层数设置为 `4`。
* MAC地址用于唯一标识网络中的每个节点，而路由器RSSI则用于参考路由器指示节点的信号强度。
* Wi-Fi信标帧包括其MAC地址和路由器RSSI值。

## [5.用户指定根节点](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)


## [6.异步上电复位](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)

* 如果网络中的某些节点异步通电（即，间隔数分钟），则网络的最终结构可能与理想的情况（所有节点同步通电）不同。

## 7.回环避免

* 环回将会破坏树型拓扑结构。


## 8.数据传输

* ESP-MESH数据包的结构及其与Wi-Fi数据帧的关系
![ESP-MESH与WIFI关系](https://img-blog.csdnimg.cn/20190923204356590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

## [9.上游流控制](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)

* ESP-MESH不支持任何下游流量控制。

## 10.双向数据流

![双向数据流](https://img-blog.csdnimg.cn/20190923204712291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
由于使用路由表，ESP-MESH能够完全在网状层上处理数据包转发。

## [11.ESP-MESH网络通道切换](https://docs.espressif.com/projects/esp-idf/en/stable/api-guides/mesh.html)

* CSA元素包含了即具有相同的“ 新通道号”和“ 通道切换计数”。


