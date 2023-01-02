# abstract关键字的使用：

##### abstract 修饰类：抽象类

**抽象类：用来模型化那些父类无法确定全部实现，而是由其子类提供具体实现的对象的类。**

1、此类不能实例化

2、抽象类中一定有构造器，便于子类实例化时调用（涉及：子类对象实例化的全过程）

3、开发中，都会提供抽象类的子类，让子类实例化，完成相关操作

##### abstract 修饰方法：抽象方法

1、抽象方法只有方法的声明，没有方法体

2、包含抽象方法的类，一定是抽象类。反之，抽象类中可以没有抽象方法。

3、子类重写父类中的所有抽象方法后，此子类才能实例化，若没有重写父类中的所有抽象方法，则此子类也是一个抽象类，需要使用**abstract**修饰。

```java
public class Test {
	public static void main(String[] args){
        
//		Per p1 = new Per();
//		p1.eat();
		
		Stu s1 = new Stu("tom",212);
		s1.eat();
	}
		
	
}

abstract class Per {
	String name;
	int age;
	
	public Per(){
		
	}
	public Per(String name , int age){
		this.name = name;
		this.age = age;
	}
	
	public void walk(){
		System.out.println("人，走路");
	}
	//抽象方法
	public abstract void eat();
}

class Stu extends Per{
    
	public Stu(String name , int age){
		super(name,age);
	}
	@Override
	//重写父类的抽象方法
	public void eat(){
		System.out.println("学生吃有营养的食物！");
	}
}
```

<!--abstract 不能修饰属性、构造器等结构，也不能修饰私有方法、静态方法、final 方法-->

