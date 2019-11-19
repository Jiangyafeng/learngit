#esp32技术手册
* **esp32内存**


##一、系统和存储器
ESP32 采用两个哈佛结构 Xtensa LX6 CPU 构成双核系统。系统中多个外设能够通过 DMA 访问片上存储器。两个 CPU 的名称分别是“PRO_CPU”和“APP_CPU”。PRO 代表“protocol(协议)”,APP 代表“application(应用)”。主要特性是地址空间，片上存储器，片外存储器，外设，DMA。
系统结构如下：
![系统结构](https://img-blog.csdnimg.cn/20190916195905210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
地址映射结构如下：
![地址映射结构](https://img-blog.csdnimg.cn/2019091620014624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
地址 0x4000_0000 以下的部分属于数据总线的地址范围,地址 0x4000_0000 ~ 0x4FFF_FFFF 部分为指令总线的地址范围,地址 0x5000_0000 及以上的部分是数据总线与指令总线共用的地址范围。
地址映射地址如下：
![地址映射](https://img-blog.csdnimg.cn/2019091620062636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
1. 片上存储器
片上存储器分为 Internal ROM、Internal SRAM、RTC FAST Memory、RTC SLOW Memory 四个部分,其容量分
别为 448 KB、520 KB、8 KB、8 KB。RTC FAST Memory 与 RTC SLOW Memory 都为 SRAM。
下面为所有片上存储器以及片上存储器的数据总线与指令总线地址段：
![片上寄存器地址映射](https://img-blog.csdnimg.cn/20190916201501924.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)

2. 片外存储器
ESP32 将 External Flash 与 External SRAM 作为片外存储器。表4 列出了两个 CPU 的数据总线与指令总线中的各段地址通过 Cache 与 MMU 所能访问的片外存储器。
![片外存储器地址映射](https://img-blog.csdnimg.cn/20190916202307234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
3. 外设
ESP32 共有 41 个外设模块。除了 PID Controller 以外,其余外设模块都可以被两个 CPU 用相同地址访问到。其中特别的是以下三个：
* 不对称 PID Controller 外设(PRO_CPU 和 APP_CPU 都只能访问自己的PID Controller,不能访问对方的 PID Controller)
* 不连续外设地址范围(外设模块 SDIO Slave 被划分为三部分)
* 存储器速度(ROM 和 SRAM 的时钟源都是 CPU_CLK,RTC FAST Memory的时钟源是 APB_CLOCK)
#二、复位和系统时钟
##复位
系统提供三种级别的复位方式,分别是 CPU 复位,内核复位,系统复位。
![系统复位](https://img-blog.csdnimg.cn/20190916204106241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
* CPU 复位:只复位 CPU 的所有寄存器。
* 内核复位:除了 RTC,会把整个 digital 的寄存器全部复位,包括 CPU、所有外设和数字 GPIO。
* 系统复位:会复位整个芯片所有的寄存器,包括 RTC。
##系统时钟
![系统时钟](https://img-blog.csdnimg.cn/20190916204316278.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
ESP32的时钟源分别来自外部晶振、内部 PLL 或震荡电路。
* CPU 时钟
CPU_CLK 为 CPU 主时钟，CPU_CLK 由 RTC_CNTL_SOC_CLK_SEL 来选择时钟源,允许选择 PLL_CLK, APLL_CLK, RTC8M_CLK, XTL_CLK
作为 CPU_CLK 的时钟源。
![CPU_CLK时钟源](https://img-blog.csdnimg.cn/20190916204801372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
* 外设时钟
外设所需要的时钟包括 APB_CLK,REF_TICK,LEDC_SCLK,APLL_CLK 和 PLL_D2_CLK。
#三、DPort寄存器
ESP32 集成了的外设,通过 DPort 寄存器控制时钟门控,功耗管理,外设以及系统核心模块的配置,可以使得系统在保持最佳性能的同时将功耗降到最低。DPort 寄存器包含了多个对应外设和模块的寄存器:系统和存储器，时钟和复位，中断矩阵，DMA，PID/MPU/MMU，APP_CPU，外设时钟门控和复位。
(1) APP_CPU 控制器寄存器
其中DPort 寄存器可用于 APP_CPU 的基本配置,例如暂停任务的执行,以及设置从 ROM code 启动后的跳转地址。
* 当 DPORT_APPCPU_RESETTING=1 时,APP_CPU 将会被复位;当 DPORT_APPCPU_RESETTING=0 时,
APP_CPU 被放开。
* DPORT_APPCPU_CLKGATE_EN=0 时,可以关闭 APP_CPU 的时钟,减少电源消耗。
* DPORT_APPCPU_RUNSTALL=1 时,可以将 APP_CPU 置于暂停(stall)状态。
* APP_CPU 从 ROM code 启动之后,会跳转到 DPORT_APPCPU_BOOT_ADDR 寄存器中的地址。

(2) 外设时钟门控和复位
其中复位和时钟门控寄存器均为高电平有效。即时钟门控寄存器置为 1 时,对应时钟被打开,置 0 为关闭。复位寄存器置为 1 时,对应外设为复位状态,置 0 时关闭复位状态,对应外设正常工作。需要注意的是,复位寄存器无法通过硬件清除。
##3.1 DMA控制器
直接存储访问 (Direct Memory Access, DMA) 用于在外设与存储器之间以及存储器与存储器之间提供高速数据传输。系统中具有 DMA 功能的模块总共有 13 个。表 3 列出了所有具有 DMA 功能的模块。
![DMA模块](https://img-blog.csdnimg.cn/20190916202013938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70 "dma")
DMA 控制器具有以下几个特点:
* [AHB总线架构](https://blog.csdn.net/ivy_reny/article/details/78144785?locationNum=3&fps=1)
* 支持半双工和全双工收发数据
* 数据传输以字节为单位,传输数据量可软件编程
* 支持 4-beat burst 传输
* 328 KB DMA 地址空间
* 通过 DMA 实现高速数据传输

![DMA引擎架构](https://img-blog.csdnimg.cn/20190916210859279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
DMA引擎架构如上图，DMA 引擎通过 AHB_BUS 将数据存入内部 RAM 或者将数据从 RAM 取出。DMA_ENGING 根据 out_link 中的内容将相应 RAM 中的数据发送出去,也可根据 in_link中的内容将接收的数据存入指定 RAM 地址空间。
out_link 与 in_link 结构相同，一个链表由 3 个字组成，这里面对于链表解释：
![链表结构](https://img-blog.csdnimg.cn/20190916211323633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
每一个字段的意义：
* owner (DW0) [31]:表示当前链表对应的 buffer 允许的操作者。
1’b0:允许的操作者为 CPU;
1’b1:允许的操作者为 DMA 控制器。
* eof (DW0) [30]:表示结束标志。
1’b0:当前链表不是最后一个链表;
1’b1:当前链表为数据包的最后一个链表。
* reserved (DW0) [29:24]:reserved。
软件不能写 1。
* length (DW0) [23:12]:表示当前链表对应的 buffer 中的有效字节数。从 buffer 中读取数据时表示能够读取
的字节数;向 buffer 中存储数据时表示已存数据的字节数。
* size (DW0) [11:0]:表示当前链表对应的 buffer 的大小。
注意:大小必须字对齐。
* buffer address pointer (DW1):buffer 地址指针。
注意:地址必须字对齐。
* next descriptor address (DW2):下一个链表的地址指针。当前链表为最后一个链表时 (eof=1),该值为 0。

用 DMA 接收数据时, 如果一帧数据长度小于给定的 buffer 长度,那么 DMA 不会接着使用这个 buffer 剩余空间。这使得 DMA_ENGING 可以用于传输任意字节数的数据。
###3.1.1 UART DMA (UDMA) 控制器
ESP32 中有 3 个 UART 接口,它们共用 2 个 UDMA 控制器。UHCIx_UART_CE(x 为 0 或者 1)寄存器用于选择 UDMA。
![UDMA 方式数据传输图](https://img-blog.csdnimg.cn/20190916212122406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
>* UART DMA 的数据包格式为(分隔符 + 数据 + 分隔符)。Encoder 用于在数据前后加上分隔符,并将数据中和分隔符一样的数据用特殊字符替换。Decoder 用于去除数据包前后分隔符,并将数据中的特殊字符进行替换为分隔符。
>* 分隔符可由 UHCIx_SEPER_CHAR 进行配置,默认值为 0xC0。数据中与分隔符一样的数据可以用 UHCIx_ESC_SEQ0_CHAR0(默认为 0xDB)和 UHCIx_ESC_SEQ0_CHAR1(默认为 0xDD)进行替换。当数据全部发送完成后,会产生 UHCIx_OUT_TOTAL_EOF_INT 中断。当数据接收完成后,会产生 UHCIx_IN_SUC_EOF_INT 中断。

###3.1.2 SPI DMA 控制器
![SPI DMA](https://img-blog.csdnimg.cn/20190916213004124.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
ESP32 SPI 除了使用 CPU 实现与外设的数据交换外,还可以使用 DMA 。共有两个 DMA 通道可供 SPI1、SPI2 和 SPI3 控制器选择,每个 DMA 通道可供一个 SPI 控制器使用,即每次可以有两个 SPI 控制器同时使用 DMA。   
SPI_DMA 接口的软件配置流程如下:
1. 首先复位 DMA 状态机和 FIFO 指针;
2. 配置 DMA 相关寄存器;
3. 配置 SPI 接口相关寄存器;
4. 使能一次 DMA 操作。
###3.1.3 I2S DMA 控制器
ESP32 有两个 I2S 接口,即 I2S0 和 I2S1。I2S0 和 I2S1 各有一个 DMA 通道。
I2S DMA 接口的软件配置流程如下:
1. 首先配置 I2S 接口相关寄存器;
2. 复位 DMA 状态机和 FIFO 指针;
3. 配置 DMA 相关寄存器;
4. 在 I2S 主机模式下,设置 I2S_TX_START 比特或者 I2S_RX_START 比特,发起一次 I2S 操作;
在 I2S 从机模式下,设置 I2S_TX_START 比特或者 I2S_RX_START 比特后等待主机发起数据传输的请求。
#四、SPI 
![SPI系统框图](https://img-blog.csdnimg.cn/20190916214421826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
ESP32 共有 4 个 SPI 控制器 SPI0、SPI1、SPI2、SPI3,用于连接支持 SPI 协议的设备。SPI0 控制器作为 cache 访问外部存储单元接口使用,SPI1 作为主机使用,SPI2 和 SPI3 控制器既可作为主机使用又可作为从机使用。
SPI 控制器在 GP-SPI 模式下,支持标准的四线全双工/半双工通信(MOSI、MISO、CS、CLK)和三线半双工通信(DATA、CS、CLK)。









