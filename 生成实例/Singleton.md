1、Singleton——只有一个实例
---
想在程序中表示某个东西只存在一个时，就会有只能创建一个的需求，例子有表示程序所运行于的那台计算机中的类、表示软件系统相关设置的类。像这样确保只生成一个实例的模式叫做Singleton模式。<br>

2、登场角色
---
* Singleton类<br>
要注意该类中方法和属性的可见性。构造函数是private的。singleton字段要用static修饰，使得singleton字段的初始化只会在第一次生成时进行，之后生成singleton类的实例时不会再初始化该字段。
getInstance()函数是public，且有static修饰符修饰，确保可以通过类名访问到该方法。<br>

3、示例程序
---
```java
public class Singleton {
	private static Singleton singleton=new Singleton();
	private Singleton()
	{
		System.out.println("生成一个实例");
	}
	public static Singleton getInstance()
	{
		//第一次调用该方法时，singleton会被初始化
    return singleton;
	}
	public static void main(String[] args) {
		Singleton s1 = Singleton.getInstance();
		Singleton s2 = Singleton.getInstance();
		if(s1==s2)
			System.out.println("两者是一个实例");

	}
}
```
4、与线程相关的问题
---
不严格的Singleton模式。以下这种写法在多线程时可能会生成多个不同的实例，不满足单例模式的要求。<br>
```java
public class Singleton {
	private static Singleton singleton=null;
	private Singleton()
	{
		System.out.println("生成一个实例");
	}
	public static Singleton getInstance()
	{
		if(singleton==null)
			singleton = new Singleton();
		return singleton;
	}
}
```
多线程测试代码<br>
* 补充——多线程实现方式（继承Thread）<br>

在子类中重新实现父类中的run()方法，然后在使用多线程的时候调用start()方法，则系统会自动调用run()方法。<br>
因为是多线程，所以程序每次的运行结果是不确定的。<br>
```java
public class Thread1 extends Thread{
 
	private String name;
	public Thread1(String name) {
		this.name=name;
	}
	//重写线程执行的操作
	public void run() {
		for(int i=0;i<10;i++) {
			System.out.println(name+" 做第 "+i+" 个俯卧撑");
		}
	}
	public static void main(String[] args) {
		Thread1 zhangsan=new Thread1("张三");
		Thread1 lisi=new Thread1("李四");
		zhangsan.start();
		lisi.start();
	}
}
```
* 不严格的单例模式多线程测试代码
```java
package Singleton;
public class Main extends Thread{
	public Main(String name)
	{
		super(name);
	}
	public void run()
	{
		Singleton obj = Singleton.getInstance();
		System.out.println(getName()+": obj = " + obj);
	}
	public static void main(String[] args) {
		new Main("A").start();
		new Main("B").start();
		new Main("C").start();
	}
}
```
执行结果，可以看到生成了三个实例<br>
```java
生成一个实例
生成一个实例
B: obj = Singleton.Singleton@3ce4fdb6
生成一个实例
C: obj = Singleton.Singleton@7c406074
A: obj = Singleton.Singleton@5fab9cb7
```
上面不严格的单例模式中，如下条件判断是线程不安全的。<br>
```java
if(singleton==null)
	singleton = new Singleton();
```
即在线程A判断```if(singleton==null)```之后，执行```singleton = new Singleton()```之前，可能会有其他线程也判断到了```singleton==null```从而生成多个实例。<br>
* 严格的单例模式<br>

添加关键字synchronized修饰静态方法，给当前类对象加锁，进入同步代码前要获得当前类对象的锁。即不存在两个对象同时进入该静态方法的情况。<br>
```java
public static synchronized Singleton getInstance()
{
	if(singleton==null)
		singleton = new Singleton();
	return singleton;
}
```
运行结果<br>
```
生成一个实例
A: obj = Singleton.Singleton@7c406074
C: obj = Singleton.Singleton@7c406074
B: obj = Singleton.Singleton@7c406074
```
5、类图
---
P46

