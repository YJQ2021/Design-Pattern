1、Template Method简介
---
Template Method是带有模板功能的模式，组成模板的抽象方法被定义在父类中，子类实现这些抽象方法。只查看父类无法知道这些抽象方法最终会进行何种具体处理（即用铅笔写还是用钢笔写），只知道父类是如何调用这些方法（即知道此模板写出来的字母是什么）。<br>
在父类中定义处理流程的框架（模板方法），在子类中实现具体的处理就叫做Template Method模式。<br>
2、登场角色
---
* AbstractClass（抽象类）<br>
两个任务：实现模板方法；声明在模板方法中用到的抽象方法。<br>
* ConcreteClass（具体类）<br>
具体实现AbstractClass角色中定义的抽象方法。这里实现的方法会在AbstractClass的模板方法中被调用。<br>
3、示例程序
---
（1）AbstractDisplay类（AbstractClass）
```java
public abstract class AbstractDisplay {
	public abstract void open();
	public abstract void print();
	public abstract void close();
	public final void display()
	{
		open();
		for(int i=0;i<5;i++)
			print();
		close();
	}
}
```
（2）CharDisplay类和StringDisplay类（ConcreteClass）
```java
public class CharDisplay extends AbstractDisplay {
	private char ch;
	public CharDisplay(char ch)
	{
		this.ch=ch;
	}
	public void open()
	{
		System.out.print("<<");
	}
	public void print()
	{
		System.out.print(ch);
	}
	public void close()
	{
		System.out.println(">>");
	}
}
```
```java
public class StringDisplay extends AbstractDisplay{
	private String str;
	private int length;
	public StringDisplay(String str,int length)
	{
		this.str=str;
		this.length=length;
	}
	public void open()
	{
		System.out.print("+");
		for(int i=0;i<length;i++)
		{
			System.out.print("-");
		}
		System.out.println("+");
	}
	public void print()
	{
		System.out.print("+");
		System.out.print(str);
		System.out.println("+");
	}
	public void close()
	{
		System.out.print("+");
		for(int i=0;i<length;i++)
		{
			System.out.print("-");
		}
		System.out.println("+");
	}
  //测试程序
	public static void main(String[] args) {
		AbstractDisplay d1 = new CharDisplay('y');
		AbstractDisplay d2 = new StringDisplay("yanjiaqi",8);
		d1.display();
		d2.display();
	}
}
```
4、小tips
---
* 使用Template Method的好处：使得逻辑处理通用化（即模板方法中的处理逻辑适用于所有的子类），在父类中编写了模板方法，而无需在每个子类中再编写相同处理逻辑的模板方法。<br>
* 理解子类与父类之间的关系<br>

子类对父类的要求：在子类中可以使用父类中定义的方法；可以通过在子类中增加方法实现新的功能；在子类中重写父类的方法可以改变程序的行为。<br>
父类对子类的要求：期待子类实现抽象方法。即子类具有实现在父类中声明的抽象方法的责任。叫做“子类责任”。<br>
* 父类与子类的协作<br>

将更多的方法实现放在父类中会让子类变得更轻松，同时也降低了子类的灵活性。反之，如果父类中实现的方法过少，子类会变得臃肿不堪，还会导致子类间的代码出现重复。<br>
5、类图
---
P29
