1、Builder模式——组装复杂的实例
===
指将一个复杂对象的构造与它的表示分离，使同样的构建过程可以创建不同的表示。即首先建造组成这个物体的各个部分，然后一步一步构建而成。即产品的组成部分是不变的，但每一部分是可以灵活选择的。<br>
* 该模式的主要优点如下：<br>

封装性好，构建和表示分离。<br>
扩展性好，各个具体的建造者相互独立，有利于系统的解耦。<br>
客户端不必知道产品内部组成的细节，建造者可以对创建过程逐步细化，而不对其它模块产生任何影响，便于控制细节风险。<br>
* 其缺点如下：

产品的组成部分必须相同，这限制了其使用范围。<br>
如果产品的内部变化复杂，则建造者也要同步修改，后期维护成本较大。<br>
2、登场角色
===
* Builder（建造者）<br>

负责定义用于生成实例的接口，准备了用于生成实例的方法。<br>
* ConcreteBuilder（具体的建造者）<br>

负责实现Builder角色接口的类，定义了在生成实例时实际被调用的方法。还定义了获取最终结果的方法（由于各个部分的表现形式不同，最终结果的形式也不同，所以该方法要定义在ConcreteBuilder中）。<br>
* Director（监工）<br>

使用Builder角色的接口来生成实例。隐藏组装的具体过程。（对象的构造，如几个窗子几个门）<br>
* Client（使用者）<br>

Builder模式的使用者会决定以什么样的表示来构造对象。以及获得最终的返回结果。<br>

3、示例代码
===
（1）Builder类<br>
决定了文档应该由哪几个要素组成。<br>
```java
public abstract class Builder {
	public abstract void makeTitle(String s);
	public abstract void makeString(String s);
	public abstract void makeItems(String[] s);
	public abstract void close();
}
```
（2）TextBuilder类<br>
使用纯文本编写文档的类。决定了纯文本条件下对象各个要素的表现形式。<br>
```java
public class TextBuilder extends Builder{
	private StringBuffer res = new StringBuffer();
	public void makeTitle(String title)
	{
		res.append("=======================\n");
		res.append("["+title+"]\n");
		res.append("\n");
	}
	public void makeString(String s)
	{
		res.append("##"+s+"\n");
		res.append("\n");
	}
	public void makeItems(String[] items)
	{
		for(int i=0;i<items.length;i++)
		{
			res.append("*"+items[i]+"\n");
		}
		res.append("\n");
	}
	public void close()
	{
		res.append("=======================\n");
	}
	public String getResult()
	{
		return res.toString();
	}

}
```
（3）Director类<br>
编写一个文档的类<br>
```java
public class Director {
	private Builder builder;
	public Director(Builder builder)
	{
		this.builder = builder;
	}
	public void construct()
	{
		builder.makeTitle("早上好");
		builder.makeString("从早上至下午");
		builder.makeItems(new String[] {"早上好","下午好"});
		
		builder.makeString("晚安");
		builder.makeItems(new String[] {"晚上好","晚安"});
		
		builder.close();				
	}
}
```
（4）Main类<br>
```java
public class Main {
	public static void main(String[] args) {
			TextBuilder builder = new TextBuilder();
			Director d = new Director(builder);
			d.construct();
			
			String res = builder.getResult();
			System.out.println(res);
	}
}
```
4、类图
===
P68


