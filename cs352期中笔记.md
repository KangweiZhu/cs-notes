# CS 352 Internet Technology Midterm I Review



## Topics will be covered in midterm I 

> 常见的三种计算机网络体系结构
>
> 1. OSI 参考模型 （从下到上是：物数网传会表应） 【法律上的国际标准 】
> 2. TCP/IP 参考模型（四层协议的体系结构）【事实上的国际标准】<-因特网采用的
>    * 从下到上是：网络接口层、网际层(网络层)、运输层、应用层
>    * 操作主机中通常都有完整的tcp/ip 协议族
>    * 路由器最高层是网际层
>    * IP是**网际协议**，是网际层的核心协议。 
>    * 两个运输层的重要协议
>      * 传输控制协议TCP   
>        * 用户数据报协议UDP





### 1. Circuit switching, message switching, packet switching

#### 1.1 Circuit switching

> ZH: 电路交换用于电话通信系统，两个用户要通信之前需要建立一条专用的物理链路，并且在整个通信过程中始终占用该链路。由于通信的过程中不可能一直在使用传输线路，因此电路交换对线路的利用率很低，往往不到 10%
>
>
> EN：
>
> 1. Control message sets up a path from origin to destination 
> 2. Return signal informs source that data transmission may proceed 
> 3. Data transmission begins 
> 4. **Entire path remains allocated to the transmission  (whether used or not)** 
> 5. When transmission is complete, source releases the circuit





#### 1.2 Message switching

> 





#### 1.3 Package switching

ZH: 每个分组都有**首部(Header)**和**尾部**，包含了源地址和目的地址等**控制信息(Control message)**，在同一个传输线路上同时传输多个分组互相不会影响，因此在同一条传输线路上允许同时传输多个分组，也就是说分组交换不需要占用传输线路。

在一个邮局通信系统中，邮局收到一份邮件之后，先存储下来，然后把相同目的地的邮件一起转发到下一个目的地，这个过程就是存储转发过程，分组交换也使用了存储转发过程。



EN: 

• Messages are split into smaller pieces called packets 

• These packets are numbered and addressed and sent  through the network one at a time 

• Pipelining



 首部的作用：

1. 包含了分组的目的地址 -> 让分组传输路径中的各个交换节点知道怎么转发分组

2. 分组交换网中的交换节点收到分组后，先将其缓存下来，然后从其首部中提取出目的地址，然后按照目的地址查找自己的转发表。找到相应的转发接口后，将分组转发出去，发到下一个交换节点。经过多次存储转发后，分组最终被转发到目的主机。

3. 目的主机收到这些分组之后，去掉这些分组的首部，将各数据段组合还原出原始报文。

![image-20230228211412451](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230228211412451.png)

### 2. Latency, throughput calculations

### 3. Application layer; DNS, Query, caching, CDN

### 4. Application layer: http variations, ftp, SMTP 

### 运输层（Transportation Layer）

> 在计算机网络中实际进行通信的真正实体，是位于通信两端主机中的进程
>
> 运输层的作用范围是**应用进程到应用进程**，即**端到端**
>
> 运输层提供**进程**间的**逻辑通信**





##### 1. 进程间基于网络的通信

公式： 主机xxx中的zzz应用 和主机xxy中的yyy应用 进行**基于网络的通信**，主机xxx中的**运输层**使用不同的端口，**对应**不同的**应用进程**，然后通过**网络层及其下层传输**应用层报文。主机xxy中的运输层通过不同的端口，将收到的应用层报文，**交付**给应用层中相应的应用进程。

![image-20230304105129925](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304105129925.png)

![image-20230304105622536](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304105622536.png)





##### 2. TCP 和 UDP 概述

> 运输层的两个重要协议
>
> ![image-20230304114224104](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304114224104.png)
>
> ![image-20230304114258344](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304114258344.png)





##### 3. TCP/IP 中的核心协议- IP（网际协议）

![image-20230304130011048](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304130011048.png)

* IP可以互联不同的网络接口，也就是互联各种异构型网络。并为其上层提供**无连接**，**不可靠**的数据报传输服务。
* TCP/IP四层体系结构的应用层中包含了大量的应用层协议。其中，有些协议需要用到可靠传输服务，例如，浏览网页，文件传输，电子邮件以及电子银行所涉及的协议。
* 应用层中的音频、视频等多媒体应用使用不可靠的传输服务。这是因为实时性是他们的首要需求，而少量传输错误对播放质量产生的影响较小。





##### 4. 运输层端口号

![image-20230304130745459](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304130745459.png)

* 端口号分为两类。一类是服务器端的端口号，另一类是客户端使用的端口号。
* 服务器端的端口号又分为两类。一类是熟知端口号，另一类是登记端口号。

![image-20230304131902293](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304131902293.png)

![image-20230304140915443](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304140915443.png)

![image-20230304141004886](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304141004886.png)

* OSPF不适用UDP/TCP进行封装，而是直接使用网际层的IP进行封装。协议字段值如图，89

### 7. 其他

#### 1. 计算机网络体系结构（五层结构【适用于教学】）

* 关键词
  1. 协议
     * 协议是控制两个对等实体在“水平方向”进行“逻辑通信”的规则的集合。
     * 三个要素：语义、语法、同步
  2. 实体
  3. 服务
     * 要实现本层协议，需要使用到下一层所提供的服务
     * 两个对等实体在水平方向的逻辑通信使得本层能向上一层提供服务。
     * 应用层的上层是用户
     * 实体看得见下层提供的服务，但是不知道实现该服务的具体协议。





##### 1. 物理层

![image-20230304050205913](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304050205913.png)





##### 2. 数据链路层

![image-20230304050303776](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304050303776.png)





##### 3. 网络层

* logical  communication between  hosts

![image-20230304050353244](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304050353244.png)





##### 4. 运输层

* **logical communication** between  processes
* 使用运输层协议如（TCP UDP）进行逻辑通信。

![image-20230304051330012](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304051330012.png)





##### 5. 应用层

![image-20230304051354647](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304051354647.png)



![image-20230304052135345](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304052135345.png)





#### 2. 术语

##### 1. PDU（Protocal Data Unit协议数据单元) / SDU (Service Data Unit服务数据单元 )

> 通讯双方交互的数据包也有专门的术语

![image-20230304103559035](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230304103559035.png)

