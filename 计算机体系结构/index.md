# 

# 计算机体系结构

## 计算机发展历史

### 冯·诺依曼体系结构

![](https://pic.imgdb.cn/item/64a24e681ddac507cc71bb16.jpg)

### 计算机发展

| 代数   | 计算机类型             | 年份   | 特点                                                       |
| ------ | ---------------------- | ------ | ---------------------------------------------------------- |
| 第一代 | 电子管计算机           | 1946年 | 并行计算，功能优先，速度很慢                               |
| 第二代 | 晶体管计算机           | 1956年 | 体积小，速度快，功耗低                                     |
| 第三代 | 集成电路计算机         | 1964年 | 体积更小，速度更快，功耗更低                               |
| 第四代 | 超大规模集成电路计算机 | 1972年 | 性能稳定，体积更小，速度更快，价格更低，可靠性强，功能丰富 |



## 计算机硬件组成

### 计算机组成

![](https://pic.imgdb.cn/item/64a24fdb1ddac507cc73da0c.jpg)

### 台式机硬件-内部

![](https://pic.imgdb.cn/item/64a2527e1ddac507cc77983a.jpg)

### 台式机硬件-外部

![](https://pic.imgdb.cn/item/64a252a91ddac507cc77dc51.jpg)

### CPU

Central Processing Unit（中央处理器/处理器）常见的电脑处理器：

Intel奔腾8086、 酷睿i5 i7 i9；AMD 锐龙

常见的手机处理器：高通 骁龙系列、苹果 A系列、海思麒麟系列、联发科 天玑系列

#### CPU的本质

![](https://pic.imgdb.cn/item/64a253081ddac507cc790cce.jpg)

**控制单元(Control Unit)：**完成数据处理整个过程中的调配工作；

**算术逻辑单元ALU(Arithmetic Logic Unit)：**完成各个指令以便得到程序最终想要的结果；

**存储单元：**负责存储原始数据以及运算结果。

#### 芯片和CPU的关系  

芯片有很多种，CPU芯片是其中一种其他还有**GPU、NPU、FPGA**芯片等等

**GPU：**Graphic Processing Unit 图形处理单元NPU：Neural Networks Process Units 神经网络处理单元FGCA：Field-Programmable Gate Array 现场可编程门

#### CPU和GPU的区别

![](https://pic.imgdb.cn/item/64a284bf1ddac507ccd3a436.jpg)

#### CPU重要参数

**核心数：**物理核心数

**线程：**超线程技术，逻辑处理器

**频率：**工作频率，1秒钟产生的脉冲信号

**32位和64位：**CPU一次能处理的位数

![](https://pic.imgdb.cn/item/64a2942e1ddac507ccf07afd.jpg)

#### CPU指令集和架构

指令是用来控制硬件的，经过编译后：01010101的电信号  

**复杂指令集（Complex Instruction Set Computer）：**每个指令做复杂动作，完成操作需要较少指令，庞大



>  代表：Intel X86



**精简指令集（Reduced Instruction Set Computer）：**每个指令做简单动作，完成操作需要很多指令，灵活



>  代表：ARM、RISC-V、MIPS



### 内存（主存）  

![](https://pic.imgdb.cn/item/64a2a0471ddac507cc06929e.jpg)

#### 内存与存储空间  

运行内存：RAM（Random Access Memory）

存储空间：ROM（Read Only Memory）  

#### 内存的工作频率  

SDRAM： 100 133 166 200 

DDR： 200 266 333 400 

DDR2： 400 533 667 800 1066

DDR3： 800 1066 1333 1600 1866 2133 

DDR4： 2133 2400 2666 3200 

DDR5：4800 5200 5600 ....

### 硬盘（外存）

#### 硬盘类型

3.5寸机械盘、 2.5寸机械盘、 2.5寸SATA固态盘、M.2固态盘

![](https://pic.imgdb.cn/item/64a2a1731ddac507cc084005.jpg)

#### 机械硬盘和固态硬盘工作原理

![](https://pic.imgdb.cn/item/64a2a1941ddac507cc0873aa.jpg)

#### 硬盘和内存的区别：

1. 读写速度
2. 作用
3. 持久存储  

### 输入输出设备  

输入设备：键盘、鼠标、麦克风、摄像头、扫描仪、数位板、游戏手柄等等；

输出设备：显示器、打印机、音响、显卡（GPU）等等。

声卡又是输入设备也是输出设备

## 计算机体系结构  

![](https://pic.imgdb.cn/item/64a2a3521ddac507cc0af1d5.jpg)
