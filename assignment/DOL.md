# DOL实例分析&编程

---

example中各文件的含义：

- **/src** 文件夹内包含2种文件：*.c,与对应的.h
是实现的模块，就是*.dot的框框的功能描述。（每个模块要实现2个接口，xxx_init和xxx_fire两个函数，分别是初始化这个模块是干了什么，以及这个模块开干的时候做什么）

- **.xml**：系统架构即模块连接方式定义
./example.xml 里面定义了模块与模块之间是怎么连接的，就是有哪些框，
哪些线，比如A框跟B框用一根线连起来，他们就在一起了。

## example1 代码分析
----

**运行example1之后的dot图，其中包含生产者、平方模块、消费者**

![ex1_dot](http://ogd1nxbhk.bkt.clouddn.com/lab3_ex1_dot.png)
**generator.c**
```c
generator.c 代码
void generator_init(DOLProcess *p) {
    p->local->index = 0;
    p->local->len = LENGTH;
}
int generator_fire(DOLProcess *p) {
    if (p->local->index < p->local->len) {
        float x = (float)p->local->index;
        //将x写到generator的端口“PORT_OUT”上
        DOL_write((void*)PORT_OUT, &(x), sizeof(float), p);
        p->local->index++;
    }
    if (p->local->index >= p->local->len) {
        DOL_detach(p); //销毁
        return -1;
    }
    return 0;
}
```

* 定义进程：每个模块都要写上xxx_fire（可能被执行无数次），至于init是可选择写或者不写的，xxx_init（只会被执行一次）。
* generator_init 是初始化函数。这里代码的意思是将当前位置置为0，设置生产者长度。这里的local指针指向的是.h文件的_local_states结构。
* generator_fire 是信号产生函数。这里的代码是：如果当前位置小于生产长度，则将x（这里是当前下标）写入到输出端，否则销毁进程。所以说就是，让这个程序被发射、开火、执行length次之后停下来。


**consumer.c**

```c
consumer.c 代码
void consumer_init(DOLProcess *p) {
    sprintf(p->local->name, “consumer”); //就是p->local->name==“consumer”
    p->local->index = 0;
    p->local->len = LENGTH;
}
int consumer_fire(DOLProcess *p) {
    float c;
    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &c, sizeof(float), p);//读consumer的端口“PORT_IN”
        printf(“%s: %f\n”, p->local->name, c); //将结构输出到命令行
        p->local->index++;
    }
    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }
    return 0;
}

```

* 定义消费者进程
* consumer_init初始化函数，含义同generator_init。
* consumer_fire信号消费函数，若当前位置小于设定长度，则读出输入端信号，并且打印；否则销毁进程（停下来）。

**square.c**

```
square.c 部分代码
int square_fire(DOLProcess *p) {
    float i;
    if (p->local->index < p->local->len) {
       // 读square的端口“PORT_IN”,将值读到i
        DOL_read((void*)PORT_IN, &i, sizeof(float), p); 
        i = i*i;     //做了个平方
        // 写square的端口“PORT_OUT”,把 I 写到那个端口
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p); 
        p->local->index++;
    }
    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }
    return 0;
}
```

* 定义平方进程
* square_fire信号处理函数，读入输入端信号i，将其平方后写出到输出端，也是重复length次之后就停止了。

### example1 运行结果

![ex1_init_res](http://ogd1nxbhk.bkt.clouddn.com/lab3_ex1_init.png)
* generator 产生0-19的整数（length为20，初始值为0）
* square 对输入进行平方操作
* consumer 输出结果

### 任务1：
修改example1，使其输出3次方数，tips:修改square.c

![ex1](http://ogd1nxbhk.bkt.clouddn.com/Ubuntu-2016-10-24-10-56-18.png)
![ex1](http://ogd1nxbhk.bkt.clouddn.com/Ubuntu-2016-10-24-10-57-02.png)
## example2 代码分析
----
**运行example2之后的dot图**

![](http://ogd1nxbhk.bkt.clouddn.com/lab3_ex2_dot.png)

example2有3个square模块

![sqr](http://ogd1nxbhk.bkt.clouddn.com/lab3_3sqr.png)
### example2 运行结果
![](http://ogd1nxbhk.bkt.clouddn.com/Ubuntu-2016-10-24-09-43-46.png)
* generator 生成0~19的整数
* square 经过三次平方操作
* consumer 输出结果

### 任务2：
修改example2，让3个square模块变成2个, tips:修改xml的iterator

![ex2](http://ogd1nxbhk.bkt.clouddn.com/Ubuntu-2016-10-24-10-57-27.png)
![ex2](http://ogd1nxbhk.bkt.clouddn.com/Ubuntu-2016-10-24-10-54-51.png)

## 体会

这次实验的实现并不难，写起来就是几行代码的事。

tips：

修改好之后，先把原先的那个build产生的东西删掉，然后重新build。

否则将永远看不见修改好的结果。
