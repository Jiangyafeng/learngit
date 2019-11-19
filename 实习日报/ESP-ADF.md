#ESP-ADF

## 1. 开发板

* [ESP32-LyraT入门](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/get-started/get-started-esp32-lyrat.html)	
* [ESP32-LyraTD-MSC入门](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/get-started/get-started-esp32-lyratd-msc.html)	
* [ESP32-LyraT-Mini入门](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/get-started/get-started-esp32-lyrat-mini.html)

[运行流程](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/get-started/index.html)
这里测试使用的是ESP32-LyraTD-MSC
![ESP32-LyraTD-MSC](https://img-blog.csdnimg.cn/20190924174823383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

## 2. ESP-ADF的开发框架
![音频开发框架](https://img-blog.csdnimg.cn/20190924175554118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
流，解码器，编码器，音频处理主要为开发框架。该API提供了一种使用诸如编解码器（解码器和编码器），流或音频处理功能之类的元素开发音频应用程序的方法。

### 2.1 音频元素
* 每个解码器，编码器，滤波器，输入流或输出流都是音频元素

### 2.2 音频管道
* 音频流水线中三个元素的组织，即HTTP读取器流，MP3解码器和I2S写入器流
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924180327166.png)

### 2.3 事件接口

* ADF提供事件接口API,API围绕FreeRTOS队列构建.

* [音频元素的类型](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/api-reference/framework/audio_common.html)

## 3. [音频流](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/api-reference/streams/index.html) 
* 负责获取音频数据然后在处理后将数据发送出去的音频元素称为音频流
* 支持以下流类型：
I2S流/HTTP流/FatFs流/RAW流/Spiffs流

* I2S流
当I2S流类型为“ writer”时，可以将数据发送到编解码器芯片或ESP32的内部DAC。为了简化配置，提供了两个宏来覆盖每种情况：

* I2S_STREAM_CFG_DEFAULT                -I2S流正在与编解码器芯片通信
* I2S_STREAM_INTERNAL_DAC_CFG_DEFAULT   -流数据发送到DAC

* FatFs流  
FatFs流是一个通用的文件系统(FAT/exFAT)模块，用于在小型嵌入式系统中实现FAT文件系统

* RAW流
RAW文件包含创建一个可视图像所必须的相机传感器数据信息。

* [Spiffs流](https://blog.csdn.net/weixin_34364135/article/details/89658525)

## 4. [编码器](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/api-reference/codecs/index.html)
* AAC Decoder
* FLAC Decoder
* MP3 Decoder
* OGG Decoder
* OPUS Decoder
* WAV Decoder and Encoder
* AMR Decoder and Encoder

## 5. [音频处理](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/api-reference/audio-processing/index.html)

* Downmix:使用Downmix合并两个音频流的内容
* Equalizer:应用十频段均衡器
* Resample Filter:更改音频采样频率，并使用“ 重采样滤波器”在单声道和两声道之间转换
* sonic:使用Sonic修改流的音调和速度

## 6. services服务
* 基于ESP32的音频设备与外部物理或虚拟设备（例如蓝牙扬声器或云服务器）接口
* [Bluetooth](https://docs.espressif.com/projects/esp-adf/zh_CN/latest/api-reference/services/bluetooth_service.html)

## 7. play_mp3||pipeline_bt_sink 工程运行以及编译工程需要注意
* 把官网的 play_mp3||pipeline_bt_sink 工程编译，可能你会编译失败，因为需要一些配置，在 make menuconfig 配置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190925154246349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190925154335388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

对于首先你得打开蓝牙配置使能：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190925154321525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

```
/esp-adf/components/audio_service/bluetooth_service.c:435 (bluetooth_service_start): enable controller failed
```
如果出现这样的问题，则需要如下：
>如果你发现你跑起来了但是没有搜索到其蓝牙耳机，也有以上报错，也许是下面不对，因为没启动蓝牙服务成功！这里尤其注意要在 make menuconfig 打开蓝牙设置蓝牙控制器模式，否则无法启动蓝牙服务：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190925154335388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

* 蓝牙测试中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019092519442279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

## 8.Esp-adf：百度语音合成测试例程
* 在测试这个例程之前，我们需要在[百度AI开放平台](https://ai.baidu.com/)创建一个[语音合成](https://ai.baidu.com/tech/speech/tts)的应用。登录用百度账号就可以，不用重新注册。选择语音识别或者语音合成
![用户注册账号](https://img-blog.csdnimg.cn/20190925163733187.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

* 创建应用

![百度语音合成注册](https://img-blog.csdnimg.cn/20190925163752700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
![百度语音合成创建成功](https://img-blog.csdnimg.cn/20190925163810439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

* 进入/esp-adf/examples/cloud_services，将pipeline_baidu_speech_mp3例程复制到测试文件夹，然后进入工程文件夹，输入make menuconfig 进入Example Configuration选项
* 依次填入wifi名称和密码，**此处的wifi和密码是用户需要提供一个能上网的router或者是个人热点**；以及我们刚才在百度平台创建应用时记录的API key和Secret Key，保存退出。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190925154246349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190925163840654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

* 通过输入make flash monitor，编译下载到开发板并开启监视。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190925163858423.png)

下载成功后，插入耳机，复位开发板，稍等一会儿就可以听到程序中的文本语音啦。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019092516391313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)