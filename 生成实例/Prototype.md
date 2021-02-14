1、Prototype模式——通过复制生成实例
===
应用场景：<br>
* 对象种类繁多，无法将他们整合到一个类当中（这一点理解的不是很明白）<br>
* 难以根据类生成实例时。生成实例的过程太过复杂，很难根据类来生成实例。这一点用工作日报的例子很好理解，比如说一个人每天工作日报的内容是没有太大变化的，如果每天都新建一个工作日报重新填写，会非常麻烦。这时候用户可以保存自己的原型模板，每次复制模板，就是原型模式。<br>
* 想解耦框架与生成的实例时。这一点用下面的示例代码来理解，Manager对所拥有的实例以（名称、对象）的方式进行了注册，那么Manager和类的耦合性就大大降低，将框架从类名的束缚中解放。<br>

2、登场角色
===
* Prototype（原型）<br>

负责定义用于复制现有实例来生成新实例的方法。一个接口或者抽象类或者实现类。存在的意义在于将复制实例生成新实例的方法抽象了出来，从而在不知道要复制的具体实例是什么类（但是该类一定实现了Product中定义的复制方法）的时候，就能完成实例的复制。<br>
* ConcretePrototype（具体的原型）<br>

实现用于复制现有实例来生成新实例的方法。
* Client（使用者）<br>

负责使用复制实例的方法生成新的实例。<br>

3、示例代码
===
（1）Product（Prototype）
注意Product继承了java.lang.Cloneable接口，则Product的实现类可以调用clone方法来自动复制实例。<br>
```java
 public interface Product extends Cloneable{
	public abstract void use(String s);
	public abstract Product createClone();
}
```
（2）UnderlinePen（ConcretePrototype）
```java
public class UnderlinePen implements Product {
	private char ulchar;
	public UnderlinePen(char ulchar)
	{
		this.ulchar=ulchar;
	}
	public void use(String s)
	{
		int length = s.length();
		System.out.println(s);
		for(int i=0;i<length;i++)
			System.out.print(ulchar);
		System.out.println();
	}
	public Product createClone()
	{
		Product p=null;
		try {
			p = (Product)clone();
		}catch(CloneNotSupportedException e)
		{
			e.printStackTrace();
		}
		return p;
	}

}
```
（3）Manager（client）
使用Product接口来复制实例<br>
```java
import java.util.HashMap;

public class Manager {
	private HashMap<String,Product> showcase = new HashMap<>();
	public void register(String name,Product product)
	{
		showcase.put(name,product);
	}
	public Product create(String name)
	{
		Product p = showcase.get(name);
		return p.createClone();
	}

	public static void main(String[] args) {
		Product p1=new UnderlinePen('~');
		Product p2 = new UnderlinePen('.');
		Manager m = new Manager();
		m.register("bo lang xian", p1);
		m.register("yuan dian", p2);
		
		Product p11 = m.create("yuan dian");
		p11.use("yanjiaqi");
		Product p22 = m.create("bo lang xian");
		p22.use("yanjiaqi");
		

	}

}
```
4、小tips
===
* Cloneable接口中没有声明任何方法，它只是用来标记“可以使用clone方法进行复制”，这样的接口被称为标记接口。<br>
* clone方法内部进行的处理是分配与要复制的实例同样大小的内存空间，接着复制字段的值。 
clone方法进行的是浅复制，即如果字段是数组，就会复制数组的引用而不是数组的值。<br>

5、类图
==
P55




