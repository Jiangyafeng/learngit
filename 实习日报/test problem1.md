# 测试环境

## 时间:2019年9月17日
## 数量: 7个
## MAC地址: (4c:11:ae:c9:dc:48)(4c:11:ae:cb:1a:e8)(a4:cf:12:33:c8:24)(4c:11:ae:ca:83:34)(3c:71:bf:ee:36:40该版本为老版本测试)(3c:71:bf:ee:36:98)(4c:11:ae:cb:0a:6c)
## 版本: ESP-IDF v3.3-beta1-328-gabea9e4 2nd stage bootloader

## 路由器信息:TP-LINK   型号:TL-WR841N(需要链接外网)

# 结果

1. 升级次数: 1171
成功:931
失败:139
成功率: 79.5%
大概下载时间平均时间:一分钟

2. 老版本数据 (对应的MAC地址：3c:71:bf:ee:36:40)
* 老版本总共升级115次，只有14次成功，成功率为12.17%

3. 新版本与老版本的对比
* 新版的成功率大幅度提高，提高了67.33%。新版本更加有效提高了OTA的升级。


4. 对于新版本的问题分析

* 4.1 中途会出现断链  
现象：出现的次数总计是22，概率为：1.9%．始终ｗｉfi链接失败，手机端也没有显示有任何新设备需要进行链接升级。现象如下：
```
 W (33195) wifi: Haven't to connect to a suitable AP now!
 I (33778) iot_task_main: sleeping for reconnect
 I (33779) JSON_TX_task_main: Received into JSON Tx queue, ready for split
 I (33780) JSON_TX_task_main: string is { "iotStatus": { "status": "SHADOW_RECONNECTING" } }
 I (33789) JSON_TX_task_main: Done sending
 E (33896) aws_iot: failed! mbedtls_net_connect returned -0x44
 ```
解决办法：进行重启就可以解决。

* 4.2 卡位
现象:出现的次数总计：56　；概率为4.8%。
```
[2019-09-17 19:27:00.113] I (112) boot: End of partition table
[2019-09-17 19:27:00.117] Fatal exception (28): LoadProhibited
[2019-09-17 19:27:00.120] epc1=0x400786f3, epc2=0x00000000, epc3=0x00000000, excvaddr=0x87298729, depc=0x00000000
```
解决方法：进行重启就可以解决。

* 4.3 出现停顿
现象：从Update accepted到ota_arbitrary_firmware有时候会出现卡顿，大概等待时间是一分钟，程序才会进行下步操作。
```
[2019-09-17 20:02:53.357] I (30956) ShadowUpdateStatusCallback: Update accepted
[2019-09-17 20:03:51.993] I (89585) ota_arbitrary_firmware: upgrading firmware from https://firmware.hatchbaby.com/prodrestPlus/restPlus_DFU_3_0_815.bin?f=g&mac=3C71BFEE369A. Try 2 of 3
```
解决办法：不用做任何操作，静等待一分钟左右，就ＯＫ。

* 4.4 其他
1. 该设备的MAC地址为(4c:11:ae:c9:dc:48)
现象：对于此芯片因为aws认证没有通过,所以无法进行下一步操作.
```
I (665820) JSON_TX_task_main: Received into JSON Tx queue, ready for split
I (665828) JSON_TX_task_main: string is { "iotStatus": { "status": "SHADOW_DISCONNECTED", "isFail": true, "reason": -26 } }
```
解决方法：需要重新获取aws认证。

2. 该设备的MAC地址为(4c:11:ae:ca:83:34)
现象：只出现过一次，这种乱码情况。
```
[2019-09-17 19:51:10.980] I (112) boot: End of partition table
[2019-09-17 19:51:10.984] ���h�K��)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�d
[2019-09-17 19:51:11.181] �)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�)�d
[2019-09-17 19:51:11.296] epc1=0x40008044, epc2=0x00000000, epc3=0x00000000, excvaddr=0x000000e5, depc=0x00000000
```
解决方法：进行重启就可以解决。








