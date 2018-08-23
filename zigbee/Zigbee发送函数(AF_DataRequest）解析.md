---
title: 2018-8-23  Zigbee发送函数(AF_DataRequest）解析 
tags: 
grammar_cjkRuby: true
---
# Zigbee发送函数(AF_DataRequest）解析和示例 
## 发送函数原型（定义在AF.c中）
``` cpp?linenums
afStatus_t AF_DataRequest( 
afAddrType_t    *dstAddr,   //目的地址结构体变量（含端点号）
endPointDesc_t  *srcEP,     //指向目的端点的端点描述符指针
unit16  cID,       //发送端点的输出簇ID      
unit16  len,         //有效数据长度               
unit8   *buf,        //数据
unit8   *transID,   //发送序列指针号，如果为缓存发送该ID增加1    
unit8   *options,      //发送选项  见下面详解         
unit8 radius,//传送跳数，通常设置为AF_DEFAULT_RADIUS
)
```
### options描述
|       名称     |   值域   |                      描述                    |
| ---------------| -------- | ----------------- |
| AF_ACK_REQUEST |   0x10   | APS应答确认请求，只使用在单播模式下          |
| AF_DISCV_ROUTE |   0x20   | 如果要使设备发现路由，将一直使能此选项       |
| AF_SKIP_ROUTE  |   0x30   | 如果使用此选项将导致设备跳过路由直接发送信息 |
| AF_EN_SECURITY |   0x40   |               保留                           |

返回值是一个afStatus_t类型的数据，发送成功将返回Zsuccess，发送失败将返回Errors。当设备要发送数据时，直接在应用层调用此函数即可。

### 发送消息示例

``` cpp?linenums
void MySendMessage(void)
{
    char messageDate[] = "Hello Word!!";
	if(AF_DataRequest(
	                  //发送的目的地址
	                  &SampleApp_P2P_DstAddr, 
					  //发送的端点描述符
	                   &SampleApp_epDesc,
					   //簇ID号，让接收方知道该消息的类型。自己定义
                       SAMPLEAPP_P2P_CLUSTERID,
					   //数据长度
                       13,
					   //指向数据的指针
                       messageDate,
					   //发送数据的ID序列号
                       &SampleApp_TransID,
					   //使用设备发现路由功能
                       AF_DISCV_ROUTE,
					   //设置路由域
                       AF_DEFAULT_RADIUS ) == afStatus_SUCCESS ))
		{
	       //发送成功后执行的代码
	    }else{
	        //发送失败后执行的代码
	   }
}
```

## 数据发送的目的地址详解

发送函数的第一个参数为目的地址的信息，目的地址的信息为另一个结构体（afAddrType_t），此结构体在AF.h中定义
### afAddrType_t 的定义
``` cpp?linenums
typedef struct
{
  union
  {
    uint16      shortAddr;
    ZLongAddr_t extAddr;
  } addr;//目的地址 16位短地址或者64位长地址
  afAddrMode_t      addrMode;//地址模式，枚举类型
  byte endPoint;  //断点信息
  uint16 panId;  // 网络PANID
} afAddrType_t;
```
#### 地址模式的枚举
``` cpp?linenums
typedef enum
{
  afAddrNotPresent = AddrNotPresent,//间接寻址
  afAddr16Bit      = Addr16Bit,//单点寻址，指定短地址
  afAddr64Bit      = Addr64Bit,//单点寻址，指定长地址
  afAddrGroup      = AddrGroup,//组寻址
  afAddrBroadcast  = AddrBroadcast//广播寻址
} afAddrMode_t;
```
1. 间接寻址
  间接寻址多用于绑定，当应用程序不知道数据包的目的地址时，将寻址模式设置为AddrNotPresent。ZStack底层将自动从堆栈的绑定表中查找目的设备的具体网络地址，这称为源绑定。如果在绑定表中找到多个设备，则向每一个设备都发送一个数据包拷贝。

2. 单点寻址
  单点寻址是标准的寻址模式，是点对点的通信，它将数据包发送给一个已知的网络地址的网络设备，单点寻址有两种方式：16位短地址（Adde16Bit）和64位长地址（Adde64Bit）
- 

3. 组寻址
当应用程序需要将数据包发送给网络上的一组设备时，使用该模式。此时，地址模式设置为afAddrGroup，并且在地址信息结构体中将目的地址设置为该组的ID号。在使用这个功能前必须在网络中定义组。

4. 广播寻址
当应用程序需要将数据包发送到网络中的每一个设备时，使用此模式。此时将地址模式设置为AddrBroadcast，地址信息的目标地址可以设置为以下几种：
- 0xFFF：如果设置为该地址，数据包将会发送到网络中的每一个设备，包括睡眠中的设备。对于睡眠中的设备，数据包将保留在父节点上，直到苏醒后主动到父节点上查询，或者知道消息超时丢失数据包。该模式为广播模式的默认值。
- 0xFFD：如果设置为该地址，数据包将会发送到除了睡眠以外的所有设备。
- 0xFFC：如果设置为该地址，数据包会发送给所有的路由器和协调器。
- 0xFFE：如果设置为该地址，应用层将不指定目标设备，而是通过协议栈读取绑定的表获得相应额度的目标设备的短地址

## 综合示例
### SampleApp.h

``` cpp?linenums


```

### SampleApp.c

``` cpp?linenums


```