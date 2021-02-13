1、Factory Method简介——将实例的生成交给子类
---
首先我们想象一个工厂，在该工厂中无论生产的产品种类是什么，都需要进行两项处理：生产产品、注册产品。这种“在父类中定义处理流程的框架（先生产再注册），在子类中实现具体处理（具体生产、具体注册）”就是上一章学到的template method。因此将template method模式用于生成实例，就变成factory method。<br>
在factory method中，父类决定实例的生成方式，但并不决定要生成的具体的类。具体的处理全部交给子类负责。<br>

2、登场角色
---
* Product（抽象产品）<br>
定义了在工厂模式中生成的实例的所持有的接口，具体的处理由子类ConcreteProduct角色决定。<br>
* ConcreteProduct（具体产品）<br>
决定了具体的产品。<br>
* Creator（抽象工厂）<br>
负责生成Product。它对具体产品一无所知，只要调用Product角色和生成实例的方法就可以生成Product的实例。不用new关键字来生成实例，而是调用生成实例的专用方法来生成实例，防止了父类和其他具体类的耦合<br>
* ConcreteCreator（具体工厂）<br>
负责生成具体的产品。<br>

3、示例程序
---
（1）Product类 （抽象产品）<br>
```java
package framework;

public abstract class Product {
	public abstract void use();
}
```
（2）Factory类（抽象工厂）<br>
抽象工厂生产抽象产品，该类使用了template method模式。声明了用于生产产品的creatProduct方法和用于注册产品的registerProduct方法。方法的实现被交给子类。在框架中，我们定义了工厂是用来“调用create方法生成Product实例”的，而create方法的实现是先调用creatProduct生成产品，再调用registerProduct注册产品。<br>
```java
package framework;

public abstract class Factory {
	public final Product creat(String owner)
	{
		Product p = creatProduct(owner);
		registerProduct(p);
		return p;
	}
	protected abstract Product creatProduct(String owner);
	protected abstract void registerProduct(Product p);
}
```
（3）Idcard类（具体产品）<br>
抽象产品Product的子类。实现了抽象方法use。<br>
```java
package idcard;
import framework.*;

public class Idcard extends Product{
	private String owner;
	Idcard(String owner)
	{
		System.out.println("制作"+owner+"的ID卡");
		this.owner=owner;
	}
	public void use()
	{
		System.out.println("使用"+owner+"的ID卡");
	}
    public String getOwner()
    {
    	return owner;
    }
	public static void main(String[] args) {
		Factory f = new IdcardFactory();
		Product p1 = f.creat("yanjiaqi");
		Product p2 = f.creat("zhangyaning");
		Product p3 = f.creat("shijiayi");
		p1.use();
		p2.use();
		p3.use();
	}
}
```
（4）IdcardFactory类（具体工厂）<br>
实现了creatProduct方法和registerProduct方法。<br>
```java
package idcard;
import java.util.*;
import framework.*;

public class IdcardFactory extends Factory {
	private List owners = new ArrayList();
	protected Product creatProduct(String owner)
	{
		return new Idcard(owner);
	}
	protected void registerProduct(Product p)
	{
		owners.add(((Idcard)p).getOwner());
	}
	public List getOwners()
	{
		return owners;
	}	
}
```

4、小tips<br>
---
* 生成实例——方法的三种实现方式<br>

（1）将creatProduct指定为抽象方法，交由子类实现<br>
（2）为creatProduct实现默认处理，如果子类没有实现该方法，将进行默认处理。<br>
```java
 public Product creatProduct(String name)
	{
		return new Product(name);
	}
```
（3）在其中抛出异常<br>
* 访问修饰符<br>

public:类外部、包外部均可访问。<br>
protected:同一包中的类、不同包中该类的子类<br>
private:只有该类内部可访问。<br>
缺省：同一包中的类<br>




 

