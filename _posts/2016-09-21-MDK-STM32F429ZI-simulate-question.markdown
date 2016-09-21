---
layout: post
title:  "MDK对STM32F429xx仿真遇到的问题"
date:   2016-01-31 14:32:39 +0800
categories: MDK
---

软件版本： MDK4.74

芯片型号： STM32F429ZI

症状：可以正常编译，编译之后使用调试的选择使用simulator进行Debug。可以进入程序断点，但是在`system_stm32f4xx.c`文件中大概360行中出现了死循环。

{% highlight c %}
 do
  {
    HSEStatus = RCC->CR & RCC_CR_HSERDY;
    StartUpCounter++;  
  } while((HSEStatus == 0) && (StartUpCounter != HSE_STARTUP_TIMEOUT));
{% endhighlight %}

经过某度大部分是以下答案：
{% highlight c %}
是Debug里面的设置有问题
主要是下面2项设置
Dialog DLL默认是DCM3.DLL
Parameter默认是-pCM3

应改为
Dialog DLL默认是DARMSTM.DLL
Parameter默认是-pSTM32F103VC
{% endhighlight %}

根据以上方法我尝试过分别改成DARMSTM.DLL和-pSTM32F429ZI是不行的。后面我注意到Output中是有错误信息的。错误的是信息大概是：
{% highlight c %}
error 65: access violation at 0x40023800 : no ′read′ permission
{% endhighlight %}

**后面找到了以下解决方法**

在地址0x40023800处访问违例，没有“读”的权限。地址0x40023800是外设寄存器地址。要使外设寄存器地址具有相应的“读”、“写”、“执行”权限，可在命令窗口中输入MAP命令(不区分大小写）。命令格式为：MAP 起始地址，结束地址 READ WRITE EXEC其中，READ表示“读”权限，WRITE表示“写”权限，EXEC表示“执行”权限，结束地址与起始地址的空间尺寸不超过128 MB，即不超过0x08000000字节。外设寄存器的存储空间分布较广，不可能在每次调试时都通过命令窗口输入MAP指令，MDK有提供在调试之前可以执行命令的文件。
{% highlight c %}
map 0x40000000,0x47ffffff readwrite
map 0x50000000,0x57ffffff readwrite
map 0xa0000000,0xa7ffffff readwrite
map 0xf0000000,0xf7f00000 readwrite
{% endhighlight %}

1.新建一个文件命名为`initmap.ini`的文件。

2.在`Options For Target`中切换到`Debug`选项卡，在Initialzation File选中刚才新建的文件保存即可

参考地址：http://blog.sina.com.cn/s/blog_46d528490101qadk.html



  
