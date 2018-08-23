---
title: 2018-8-23  Zigbee发送函数(AF_DataRequest）解析 
tags: 
grammar_cjkRuby: true
---
# Zigbee发送函数(AF_DataRequest）解析 

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
    char messageDate[] = "Hello Word!!"
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


2. 单点寻址


3. 组寻址


4. 广播寻址