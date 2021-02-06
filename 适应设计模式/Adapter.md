Adapter
===
1、适用场景<br>
---
有一些类已经充分测试过了，我们编程时会更愿意复用这些现有的类（即已有交流100伏特电源）。现有的类无法直接使用（这里我理解为，需要的接口和现有类的接口不一致，或者需要在现有类的基础上做一些功能扩充，在或者像书上说的现有类只知道接口而代码未知，无法做简单更改）（需要直流12伏特）。这时候我们就需要一个适配器填补“现有的程序”和“需要的程序之间的差异”（适配电源）。<br>
因为java不支持多继承，所以适配器和“现有类”以及“需求类”之间的关系，可以有两种模式：
* 类适配器模式<br>
即适配器是“现有类”的子类，而实现“需求类”这个接口<br>
* 对象适配器模式<br>
即适配器持有“现有类”的引用，而继承了“需求类”<br>
无论哪一种模式，都可以看到，适配器需要利用“现有类”实现“需求类”的需求。而面向对象中类与类“利用”这种关系，就可以通过引用或者继承两种方式来实现，所以就有两种适配器模式。<br>
2、登场角色
* Target（需求）<br>
负责定义所需的方法
* Adaptee（被适配）<br>
Adaptee是一个持有既定方法的角色。需要注意的一点是，如果Adaptee角色中的方法就是Target中所需要的，就不需要接下来的Adapter了。<br>
* Adapter （适配）<br>
使用Adaptee的方法来满足Target的需求。<br>
2、示例代码（只实现了类适配器模式）
---
Banner类（被适配）<br>
```java
public class Banner {
	private String string;
	public Banner(String string)
	{
		this.string = string;
	}
	public void showWithParen()
	{
		System.out.println("("+string+")");
	}
	public void showWithAster()
	{
		System.out.println("*"+string+"*");
	}
}
```
Print接口（需求）
```java
public interface Print {
	public abstract void printWeek();
	public abstract void printStrong();
}
```
PrintBanner类（适配器）
```java
public class PrintBanner extends Banner implements Print{
	public PrintBanner(String string)
	{
		super(string);
	}
	public void printWeek()
	{
		showWithParen();
	}
	public void printStrong()
	{
		showWithAster();
	}
}
```
3、类图<br>
---
书P19


