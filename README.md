STboot
======
欢迎使用STboot,这是一个基于lua的stm8的串口下载程序，通过这段小程序你可以方遍的实现对stm8进行无限次的串口固件下载或升级。

Table of Contents
=================

* [Description](#description)
* [Summary](#summary)
    * [碰到的坑](#碰到的坑)
    * [参考资源](#参考资源)

Description
===========
如何使用：

1、你要打开stm8单片机的bootloader功能，具体过程请参考[STM8用串口下载及调试从入门到精通.pdf](https://github.com/regocxy/STboot/blob/master/doc/STM8%E7%94%A8%E4%B8%B2%E5%8F%A3%E4%B8%8B%E8%BD%BD%E5%8F%8A%E8%B0%83%E8%AF%95%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E7%B2%BE%E9%80%9A.pdf)

2、启动lua程序

```bash
git clone https://github.com/regocxy/STboot.git
cd STboot
lua stb.lua
```
3、复位一下stm8单片机，顺利的话固件固件将自动进行下载，默认下载的固件是res/test.s19。

Summary
=======

碰到的坑
------

1、luars232库（我用的库非最新）存在bug，初始化完成之后要先进行read，不然直接write的话，会出现写数据丢失的情况。

2、stm8 bootloader使用的是echo模式，即一应一答，所以，每次对单片机下完指令后要确保把返回值都回复了回去，不然，会出现bootloader工作不正常的情况。

3、向单片机进行写和擦除操作前，一定要先向单片机下载[routine文件](https://github.com/regocxy/STboot/blob/master/res/E_W_ROUTINEs_32K_ver_1.3.s19)(注意不同型号的stm8单片机，以及不同版本的bootloader，要选择其相应的routine文件)，具体操作过程参考[stm8bootloader.pdf](https://github.com/regocxy/STboot/blob/master/doc/stm8bootloader.pdf)。

4、stm8进行擦除操作时，除了先下载[routine文件](https://github.com/regocxy/STboot/blob/master/res/E_W_ROUTINEs_32K_ver_1.3.s19)，还要对0x4000地址进行read操作，不然擦除操作将会被拒绝

5、stm8 内置flash，写块大小为128bytes，以128bytes为一块进行下载，将能极大的提高下载速度

6、注意如果你开启了读保护，bootloader将会自动跳过，具体参考[stm8bootloader.pdf](https://github.com/regocxy/STboot/blob/master/doc/stm8bootloader.pdf)。

参考资源
------

1、[stm8bootloader.pdf](https://github.com/regocxy/STboot/blob/master/doc/stm8bootloader.pdf)

2、[STM8用串口下载及调试从入门到精通.pdf](https://github.com/regocxy/STboot/blob/master/doc/STM8%E7%94%A8%E4%B8%B2%E5%8F%A3%E4%B8%8B%E8%BD%BD%E5%8F%8A%E8%B0%83%E8%AF%95%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E7%B2%BE%E9%80%9A.pdf)

3、stmflashloader源码

4、[s19文件格式](http://blog.chinaunix.net/uid-22915173-id-249854.html)

[Back to TOC](#table-of-contents)

