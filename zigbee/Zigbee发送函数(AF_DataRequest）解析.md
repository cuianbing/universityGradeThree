---
title: 2018-8-23  Zigbee发送函数(AF_DataRequest）解析 
tags: 
grammar_cjkRuby: true
---
## Zigbee发送函数(AF_DataRequest）解析 

### 发送函数原型（定义在AF.c中）
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
#### options描述
|       名称     |   值域   |                      描述                    |
| ---------------| -------- | ----------------- |
| AF_ACK_REQUEST |   0x10   | APS应答确认请求，只使用在单播模式下          |
| AF_DISCV_ROUTE |   0x20   | 如果要使设备发现路由，将一直使能此选项       |
| AF_SKIP_ROUTE  |   0x30   | 如果使用此选项将导致设备跳过路由直接发送信息 |
| AF_EN_SECURITY |   0x40   |               保留                           |

返回值是一个afStatus_t类型的数据，发送成功将返回Zsuccess，发送失败将返回Errors。当设备要发送数据时，直接在应用层调用此函数即可。

#### 发送消息示例

``` cpp?linenums
vvoid MySendMessage(void)
{
    char messageDate[] = "Hello Word!!"
	if(AF_DataRequest( &SampleApp_P2P_DstAddr, 
	                   &SampleApp_epDesc,
                       SAMPLEAPP_P2P_CLUSTERID,
                       4,
                       str,
                       &SampleApp_TransID,
                       AF_DISCV_ROUTE,
                       AF_DEFAULT_RADIUS ) == afStatus_SUCCESS )){
	
	}else{
	
	}
}
```