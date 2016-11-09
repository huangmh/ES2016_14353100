# ES2016_14353100(DOL)

------------
## Description
---------
Distributed Operation Layer(DOL)是一种用于编码分布式操作的应用框架，DOL可以自动地将应用映射到多和处理器的平台上，DOL共包含以下三个基本组成成分：


- **DOL Application Programming Interface**

----
它规定了一系列计算和交流规则，这些规则可以成功实现编程的分布化和SHAPES平台上应用的平行化。DOL借助自己所确定的规则确保应用开发者在不了解底层框架结构的情况下也可以编写程序，
- **DOL Functional Simulation**

----
这部分是为了让应用者可以在编程与调试时也可以测试自己的应用性能。除了可以验证所开发应用的功能之外，该模拟功能还可以获得在应用层的性能参数。
- **DOL Mapping Optimization**

----
这部分是为了实现由一个应用到SHAPES结构平台上的最佳映射优化。第一步中，在抽象层，一个描述应用和结构的XML格式已经被定义！必须注意的一点是所有来进行精确性能估算的必要信息都必须包含在内。


## How to Install
------------

## install

1. 配置环境

```
   sudo apt-get update      
   sudo apt-get install g++
   sudo apt-get update
   sudo apt-get install ant
   sudo apt-get install openjdk-7-jdk
   sudo apt-get install unzip
```

2. 解压文件

```
sudo mkdir dol #新建dol的文件夹
sudo unzip dol_ethz.zip -d dol #将dolethz.zip解压到 dol文件夹中
sudo tar -zxvf systemc-2.3.1.tgz 解压systemc
```

3. 编译systemc

```
cd systemc-2.3.1 #进入systemc-2.3.1的目录
sudo mkdir objdir #新建一个临时文件夹objdir
cd objdir #进入该文件夹objdir
sudo ../configure CXX=g++ --disable-async-updates #运行configure

```

* 运行configure之后的截图

![运行configure截图](http://7xstaa.com1.z0.glb.clouddn.com/1.png)

```
sudo make install #编译
cd ..
ls #查看
```

* 你可以看到的文件目录


![](http://7xstaa.com1.z0.glb.clouddn.com/2.png)

```
sudo pwd #查看当前路径
```

![](http://7xstaa.com1.z0.glb.clouddn.com/3.png)

* 去dol文件中找到build_zip.xml，用编辑器打开。

![](http://7xstaa.com1.z0.glb.clouddn.com/4.png)

![](http://7xstaa.com1.z0.glb.clouddn.com/5.png)

找到

```
<property name="systemc.inc" value="YYY/include"/>
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
```

把YYY改成上面得到的当前路径。

![](http://7xstaa.com1.z0.glb.clouddn.com/6.png)

```
sudo ant -f build_zip.xml all #编译
```

![](http://7xstaa.com1.z0.glb.clouddn.com/7.png)

OK,成功啦。


## Experiment experience  
----------------

这次实验只是配置一下环境，但是真真真真好难啊。

Java要小心配。

这是忠告。
<<<<<<< HEAD
=======
>>>>>>> wrote a readme file
>>>>>>> lab3
