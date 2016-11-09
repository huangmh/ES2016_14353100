# Deadlock

---

死锁就是两个或者多个进程，互相请求对方占有的资源。

## 死锁形成的4个条件

* 互斥条件：一个资源每次只能被一个进程使用
* 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放
* 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺
* 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系


## 死锁

设计以下代码来人为制造死锁：
```j
class A{
	synchronized void methodA(B b){
		b.last();
	}
	
	synchronized void last(){
		System.out.println("Inside A.last()");
	}

}

class B{
	synchronized void methodB(A a){
		a.last();
	}
	
	synchronized void last(){
		System.out.println("Inside B.last()");
	}

}

class Deadlock implements Runnable{
	A a=new A();
	B b=new B();

	Deadlock(){
		Thread t = new Thread(this);
		int count = 10000;
		
		t.start();
		while(count-->0);
		a.methodA(b);
	}
	public void run(){
		b.methodB(a);
	}
	
	public static void main(String args[]){
		new Deadlock();
	}
}
```    

![result](http://ogd1nxbhk.bkt.clouddn.com/lab4.png)

A，B两个类都有一个synchronize方法，分别是methodA和methodB。

在methodA和methodB中，调用的是传入的那个类的实例的last。

看看last，实际上，它只做了输出到屏幕这一项工作。

新建一个线程是t，t.start()把t加入到线程队列中。

要是按照我们的代码的顺序来走。主线程开始while循环，之后调用a的methodA方法。methodA中调用了b.last()方法，这样子，很快就会有输出

>Inside B.last()
Inside A.last()

**上图的169次就是这样**

但是由于java线程调度是采用时间片调度，也就是，一个线程跑了一段时间，CPU资源就会给到另一个线程。

所以每个线程都有可能在运行到一半发生时间片耗尽的情况。

如果我们调节count值就会达到这个效果：count越大，在while循环里消耗的时间就越多，分配给methodA的时间就越少。

时间少会有两种情况：一种是在没开始执行就耗尽了时间片，导致t线程在主线程获得下一次时间片之前运行完毕，这样会导致A，B输出顺序颠倒.

>Inside A.last()
Inside B.last()

**上图的168次就是这样**

另外一种情况是时间片在执行methodA执行到一半的时候就耗尽了，此时主线程占用了A的synchronized方法的信号量，却没有执行下一步，这时跳到了t线程，t线程获得B方法的信号量调用methodB，结果发现A信号量被占用，于是t被阻塞，又到主线程被调度，自此形成了循环等待，形成死锁.

**程序在第175次循环形成了死锁**

## 心得

这次没什么tips，就是直接用就好了。

听说有人没有形成死锁。

建议重来一遍。






