# ESP-who

##种类

* ESP-WROVER-KIT或ESP-EYE Devkit 

使用的摄像头驱动是ov2640或者是ov3660芯片。
* MTMN(人脸检测模型)
1. p-net 提案网络
2. R-net　精炼网络
3. O-net 输出网络

1. 在Web服务器网页获取摄像头

    主要调用的是wifi,http,camera的API,wifi中有三种模式station mode，soft-AP mode, station + soft-AP mode。

2. 单ESP32芯片摄像头的人脸检测，在串行中端中显示检测结果

3. ESP-EYE的面部检测和识别

    这里面增加了语音唤醒功能。
4. 



