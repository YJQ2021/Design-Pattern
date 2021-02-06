Iterator模式  一个一个遍历
===
1、登场角色
---
* Aggregate接口（集合）<br>
表示需要遍历集合的接口，实现了该接口的类可以成为一个可以保存多个元素的集合。<br>
如示例程序中，书架是一个可以放书的集合，并且有遍历书架的需求，所以书架类就是Aggregate接口的实现类。<br>
* Iterator接口（迭代器）<br>
Iterator接口用于遍历集合中的元素。相当于循环语句中的循环变量。<br>
最简单的Iterator接口包含hasnext()和next()方法<br>
* ConcreteAggregate（具体的集合）<br>
Aggerate接口的实现类。创建具体的Iterator，即concreteIterator<br>
* ConcreteIterator （具体的迭代器）
实现Iterator定义的接口 <br>
2、示例程序
---
* Aggregate和Iterator接口<br>
```java
public interface Aggregate {
	public abstract Iterator iterator();
}

public interface Iterator {
	public abstract boolean hasNext();
	public abstract Object next();
}
```
* BookShelf类（ConcreteAggregate）<br>
除Bookshelf类本身有的属性和方法外，还实现了Aggregate接口，可以返回ConcreteIterator。Bookshelf的调用者可以根据返回的ConcreteIterator来遍历Bookshelf集合中的元素，而不直接访问Bookshelf。<br>
也就是说当Bookshelf中Book对象的存储方式发生变化，比如从数组存储转为arraylist存储时，Bookshelf的调用者无需更改对Bookshelf的访问方式。<br>
这一点是使用迭代器模式的最大优点<br>
```java
public class BookShelf implements Aggregate{
	private Book[] book;
	private int last;
	public BookShelf(int max)
	{
		this.book = new Book[max];
		this.last = 0;
	}
	public Book getBookAt(int index)
	{
		return this.book[index];
	}
	public void appendBook(Book book)
	{
		this.book[last++]=book;
	}
	public int getLength()
	{
		return last;
	}
	public Iterator iterator()
	{
		return new BookShelfIterator(this);
	}
}
```
* BookShelfIterator类（ConcreteIterator）<br>
实现了Iterator接口，完成了对Bookshelf的遍历。可以看到BookShelfIterator类也只是调用了BookShelf对外提供的已封装的方法。与BookShelf中Book具体的存储方式无关。<br>
```java
public class BookShelfIterator implements Iterator {
	private BookShelf bookshelf;
	private int index;
	public BookShelfIterator(BookShelf bookshelf)
	{
		this.bookshelf = bookshelf;
		this.index = 0;
	}
	public boolean hasNext()
	{
		if(index >= bookshelf.getLength())
			return false;
		return true;
	}
	public Object next()
	{
		return bookshelf.getBookAt(index++);
	}
}
```
* Book类和测试类
```java
public class Book {
	private String name;
	public Book(String name)
	{
		this.name=name;
	}
	public String getName()
	{
		return name;
	}
}
```
```java
public class Main {

	public static void main(String[] args) {
		BookShelf shelf = new BookShelf(4);
		shelf.appendBook(new Book("yanjiaqi"));
		shelf.appendBook(new Book("zhangyaning"));
		shelf.appendBook(new Book("shijiayi"));
		shelf.appendBook(new Book("wuyiwen"));
		Iterator it = shelf.iterator();
		while(it.hasNext())
		{
			Book book = (Book)it.next();
			System.out.println(book.getName());
		}
	}
}
```
3、类图<br>
书 P8
4、小的tips<br>
* Iterator模式关键在于将实现和遍历分离开来，遍历的方式并不依赖于具体的实现。当实现发生改变时，我们仍可以利用返回的接口采用一致的方式遍历集合。
而设计模式的作用就是帮助我们编写可复用的类。“可复用”就是当某一个组件发生改变时，不需要对其他的组件进行修改或者只需要进行很小的修改，也即“对修改关闭”。
* 抽象类和接口似乎让代码变得冗余繁琐，但是如果只使用具体的类来解决问题，很容易导致类之间的强耦合，不利于类的复用。所以要尽量做到“不要只使用具体类来编程，而要多使用抽象类和接口”。<br>
* Iterator的种类多种多样，可以从前向后遍历、从后向前或者跳跃遍历。因此可以为一个ConcreteAggregate编写多个ConcreteIterator。<br>

