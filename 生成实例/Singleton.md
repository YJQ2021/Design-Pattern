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

4、类图
---
P46

