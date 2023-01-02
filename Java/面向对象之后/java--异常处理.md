# 异常：

程序执行中发生的不正常的情况称为 “异常”：

**1、Error：**java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重情况。

**2、Exception：**其他因为编程错误或偶然的外在因素导致的一般性问题，可以试用针对性代码进行处理。

例如：数组角标越界、空指针异常、网络中断等等



### 一、异常体系结构：

java.lang.Throwable

​	》java.lang.Error：一般不写针对性代码进行处理。

​	》java.lang.Exception：可以进行异常的处理

​			》编译时异常：执行**javac.exe**命名时，可能出现的异常

​			》运行时异常：执行**java.exe**命名时，出现的异常

##### 各种异常种类：

```java
	//运行时异常
	// 1.NullPoionterException（空指针异常）
	@Test
	public void test1() {
		int[] arr = null;
		System.out.println(arr[3]);

		String string = "abc";
		string = null;
		System.out.println(string.charAt(0));
	}
	// 2.ArrayIndexOutBoundsException(数组角标越界）
	@Test
	public void test2() {
		int[] arr = new int[10];
		System.out.println(arr[10]);

		String string = "abc";
		System.out.println(string.charAt(3));
	}
	// 3.ClassCastException(类型转换异常）
	@Test
	public void test3() {
		Object object = new Date(1);
		String string = (String) object;
	}
	// 4.NumberFormatException(数值转换异常）
	@Test
	public void test4() {
		String string = "12";
		string = "abc";
		int num = Integer.parseInt(string);
	}
	// 5.InputMismatchException(输入不匹配）
	@Test
	public void test5() {
		Scanner scanner = new Scanner(System.in);
		int score = scanner.nextInt();// 输入数字对，输入其他不对
		System.out.println(score);
		//scanner.close();
	}
	// 6.ArithmeticException(算数异常）
	@Test
	public void test6() {
		int a = 10;
		int b = 0;
		System.out.println(a / b);
	}
	/************************************************/
	//编译时异常
	@Test
	//Unhandled exception type FileNotFoundException
	public void test7() {
		File file = new File("hello.txt");
		FileInputStream fis = new FileInputStream(file);
		
		int date = fis.read();
		while(date != -1){
			System.out.print((char)date);
			date = fis.read();
		}
		fis.close();
	}

```



### 二、异常处理：抓抛模型

#### 	过程一：

“抛”：程序正常执行的过程中，一旦出现异常，就会在异常代码出生成一个对应异常类的对象。并将此对此对象抛出，一旦抛出对象以后，其后面的代码就不再执行。

关于异常对象的产生：1、系统自动生成的异常对象

​						 	 	 	2、手动生成一个异常对象，并抛出（**throw**）

```java
//手动生成一个异常对象，并抛出（throw）
public class ExceptionTest {
	public static void main(String[] args) {
		try {
			Stu stu = new Stu();
			stu.regist(-1001);
			System.out.println(stu);//不执行
		} catch (Exception e) {
			//e.printStackTrace();
			System.out.println(e.getMessage());//输入的数据非法
		}
	}
}

class Stu{
	private int id;
	public void regist(int id) throws Exception{
		if(id > 0){
			this.id = id;
		}else{
			//System.out.println("输入的数据非法！");
			//手动抛出异常对象
			throw new Exception("输入的数据非法！");
		}
	}
	@Override
	public String toString() {
		return "Stu [id=" + id + "]";
	}
}
```



#### 	过程二：

“抓”：可以理解为异常的处理方式：

##### 		1、try-catch-finally

​	说明：（1）、**finally**不一定要写，根据实际情况选择。

​				（2）、使用**try** 将可能出现的异常代码包装起来，在执行的过程中，一旦出现异常，就会生产一个对应异常类的对象，去**catch**中进行匹配。

​				（3）、一旦**try**中的异常对象匹配到，某一个**catch**时，就进入中进行异常的处理。一旦处理完成，就跳出**try-catch**结构（在没有**finally**的情况下），继续执行其后的代码。

​				（4）**catch**中的异常类型如果没有子父类关系，则声明顺序无所谓，若有满足子父类关系，则子类一定声明在父类的上面，否则会报错。

​				（5）常用的异常对象处理的方式：**String getMessage();** 																					**printStackTrace();**

​				（6）在**try**结构中声明的变量，出了**try** 结构后就不能在被调用。

​				（7）**try-catch-finally**可以嵌套使用

**finally说明：**

（1）、**finally**中声明的是一定会被执行的代码。即使**catch**中出现了异常，**try**或者**catch**中有**return**语句。

（2）、像数据库链接、输入输出流、网络编程Socket等资源、JVM是不能自动的回收的，我们需要自己动手进行资源的释放。此时的资源释放就要声明在**finally**中。

**体会1：**使用**try-catch-finally**处理编译时异常，是得程序再编译时就不会报错，但运行时仍可能报错。相当于我们使用**try-catch-finally**讲一个编译时可能出现的异常，延迟到运行时再出现。

体会2：开发中，由于运行时异常比较常见，所以我们通常就不针对运行时异常编写**try-catch-finally**了，针对编译时异常，我们一定要考虑异常的处理。

```java
try{
	catch(异常类型1 变量名1){
        //处理异常方式1
    }
    catch(异常类型2 变量名2){
        //处理异常方式2
    }
    ......
    //finally不一定要写，根据实际情况选择
    finally{
        //一定会执行的代码
    }
}
```



```java
	@Test
	public void test1() {
		String string = "12";
		string = "abc";
		try{
			int num = Integer.parseInt(string);
			System.out.println("hello----1");//不会执行
		}catch(NumberFormatException e){//常用异常对象处理方式
			//System.out.println("出现数值转换异常，不要着急");
			//System.out.println(e.getMessage());
			//e.printStackTrace();
		}
		catch(NullPointerException e){
			System.out.println("出现空指针异常，不要着急");
		}catch(Exception e){
			System.out.println("出现异常了，不要着急");
		}
		System.out.println("hello----2");//异常处理了就会执行
	}

/**********************finally***************************/
	@Test
	public void TestMethod(){
		int num = method();
		System.out.println(num);
        //输出：2 我一定会被执行
	}
	
	public int method() {
		try{
			int[] arr = new int[10];
			System.out.println(arr[10]);
			return 1;
		}catch(ArrayIndexOutOfBoundsException e){
			e.printStackTrace();
			return 2;
		}finally{
			System.out.println("我一定会被执行");
			//return 3;
		}
	}
```



##### 	2、**throws** + 异常类型

（1）、**“ throws + 异常类型 ”** 写在方法的声明处。指明此方法执行时，可能会抛出的异常类型。一旦当方法体执行时，出现异常，会在异常代码处生成一个异常类的对象，此对象满足throws后异常类型时，就会被抛出。异常代码后续的代码，就不会再执行。

（2）、体会：**try-catch-finally**：真正的将异常给处理掉了。

​				  		**throws**：只是将异常抛给方法的调用者。并没有真正将异常处理掉。

```java
public static void main(String[] args){
		try{
			method2();//发现异常并处理
		}catch(IOException e){
			e.printStackTrace();
		}
	}
	
	public static void method2() throws IOException{
		method1();//发现异常，抛给main
		System.out.println("到我了嘛");//有异常，没到你
	}
	
	public static void method1() throws FileNotFoundException , IOException{
		File file = new File("hello.txt");//文件不存在
		System.out.println("到我了嘛");//还没发现异常，会到你
		FileInputStream fileInputStream = new FileInputStream(file);//发现异常，抛给method2，后面的语句不再执行
        
		int data = fileInputStream.read();
		while(data != -1){
			System.out.println((char)data);
			data = fileInputStream.read();
		}
		
		fileInputStream.close();
		System.out.println("到我了嘛");
	}
```



> 对方法重写规则的解释：（抛出的异常不能大于父类中抛出的异常(可以等于父类中抛出的异常）
>

```java
public class ExceptionTest {
	public static void main(String[] args) {
		ExceptionTest test = new ExceptionTest();
		test.display(new SubClass());//多态性，到display方法
	}
	
	public void display(SuperClass s){
			try {
				s.method();//发现s可能存在IOException的异常
			} catch (IOException e) {
			/*这里的异常是根据s的可能存在的异常来处理的,若s子类重写方法后抛出的异常大于父类的异常则这里不能被处理*/
                e.printStackTrace();
			}
	}
	
}

class SuperClass{
	public void method() throws IOException{
		
	}
}

class SubClass extends SuperClass{
	public void method() throws FileNotFoundException{
		
	}
}
```



### 三、如何选择两种异常处理方式：

1、如果父类中被重写的方法没有**throws** 方式处理异常，则子类重写的方法也不能用**throws** ，意味着如果子类重写的方法中有异常，必须使用**try-catch-finally** 方式处理。

2、执行的**方法a**中，先后又调用了另外的几个**方法、b、c**，这几个方法是递进关系执行的（c会用到b，b会用到a），则建议这几个方法用**throws** 方式进行处理。而执行的**方法a**可以考虑使用**try-catch-finally**进行处理。



### 四、如何自定义异常类：

1、继承与现有的异常结构：**RuntimeException、Exception**

2、提供全局常量：**serialVersionUID**

3、提供重载的构造器

```java
class Stutest {
	// 手动生成一个异常对象，并抛出（throw）
	public static void main(String[] args) {
		try {
			Stu stu = new Stu();
			stu.regist(-1001);
			System.out.println(stu);// 不执行
		} catch (Exception e) {
			System.out.println(e.getMessage());
            // 输入的数据非法
		}
	}
}

class Stu {
	private int id;
	public void regist(int id) throws Exception {
		if (id > 0) {
			this.id = id;
		} else {
			throw new ExceptionTest("不能输入负数！");
		}
	}
}

//自定义异常类
class ExceptionTest extends Exception{//1、继承与现有的异常结构
    
    static final long serialVersionUID =   				-7034897190745766939L;//2、提供全局常量：serialVersionUID
    
    //3、提供重载的构造器
    public ExceptionTest(){
    	
    }
    public ExceptionTest(String msg){
    	super(msg);
    }
}
```



练习：

```java
public class Exercise {
    //空参构造器
	public Exercise() {}
	// main函数
	public static void main(String[] args) {
		Exercise testException = new Exercise();
		try {
			testException.testEx();
			System.out.println("嘛了");
		} catch (Exception e) {
			System.out.println("进来了吗？");//没有进来
			e.printStackTrace();
		}
	}

	boolean testEx() throws Exception {
		boolean ret = true;
		try {
			ret = testEx1();
			System.out.println( "ret = " + ret);
			
		} catch (Exception e) {
			System.out.println("testEx, catch exception");//不执行
			ret = false;
			throw e;
		} finally {
			System.out.println("testEx, finally; return value=" + ret);
			return ret;
		}
	}

	boolean testEx1() throws Exception {
		boolean ret = true;
		try {
			ret = testEx2();
			System.out.println("ret = " + ret);
			if (!ret) {
				System.out.println("进来if里面了");
				return false;
			}
			
			System.out.println("testEx1, at the end of try");//不执行
			return ret;
		} catch (Exception e) {
			System.out.println("testEx1, catch exception");//不执行
			ret = false;
			throw e;
		} finally {
			System.out.println("testEx1, finally; return value=" + ret);
			System.out.println();
			return ret;
		}
	}

	boolean testEx2() throws Exception {
		boolean ret = true;
		try {
			int b = 12;
			int c;
			for (int i = 2; i >= -2; i--) {
				c = b / i;//i=0,c出现异常
				System.out.println("i=" + i);
			}
			return true;
		} catch (Exception e) {
			System.out.println("testEx2, catch exception");
			ret = false;
			throw e;
			
		} finally {
			System.out.println("testEx2, finally; return value=" + ret);
			System.out.println();
			return ret;
		}
	}
}
/*
结果：
i=2
i=1
testEx2, catch exception
testEx2, finally; return value=false

ret = false
进来if里面了
testEx1, finally; return value=false

ret = false
testEx, finally; return value=false
嘛了
*/
```

修改练习1:

```java
public class Exercise {

	public Exercise() {

	}

	// main函数
	public static void main(String[] args) {
		
		Exercise testException = new Exercise();

		try {
			
			testException.testEx();
			System.out.println("到我了，说明异常被testEx或者testEx1处理了，就不用执行后面的语句");
			
		} catch (Exception e) {
			System.out.println("进来了吗？");
			System.out.println("哈哈，进来了");
			e.printStackTrace();
		}
	}

	void testEx() throws Exception {
		boolean ret = true;
		try {
			testEx1();
			System.out.println( "ret = " + ret);
			
		} catch (Exception e) {
			System.out.println("testEx, catch exception");
			ret = false;
			throw e;
		} finally {
			System.out.println("testEx, finally; return value=" + ret);
		}
		
		System.out.println("到我了,说明异常被testEX1或者我自己处理了");
		System.out.println();
	}

	void testEx1() throws Exception {
		boolean ret = true;
		try {
			int b = 12;
			int c;
			for (int i = 2; i >= -2; i--) {
				c = b / i;//i=0,c出现异常
				System.out.println("i = " + i);
			}
		} catch (Exception e) {
			System.out.println("testEx2, catch exception");
			ret = false;
			throw e;
			
		} finally {
			System.out.println("testEx2, finally; return value=" + ret);
			System.out.println();
		}
		
		System.out.println("到我了,说明异常被我自己处理了，所以就执行了finally后面的语句");
		System.out.println();
	}
```



### 五、补充：

#### try-catch-finally 语句中有return情况：



##### 1、try中有return语句和最后有return：

```java
public class testExcReurn {
    public int add(int a,int b) {
        try {
            return a+b;
        }catch(Exception e){
            System.out.println("catch语句块");
        }finally {
            System.out.println("finally语句块");
        }
        return 0;
    }
    public static void main(String[] args) {
        testExcReurn t = new testExcReurn();
        System.out.println("和="+t.add(9, 34));
    }
}
/*
结果：
finally语句块
和=43
解释：在try中执行到return语句时，不会真正的return，即只是会计算return中的表达式，之后将结果保存在一个临时栈中，接着执行finally中的语句，最后才会从临时栈中取出之前的结果返回。也就是说return在finally后面执行。
*/
```



##### 2、try和finally都有return语句：

```java
public class testExcReurn {
    public int add(int a,int b) {
        try {
            return a+b;
        }catch(Exception e){
            System.out.println("catch语句块");
        }finally {
            System.out.println("finally语句块");
            return 0;
        }
    }
    public static void main(String[] args) {
        testExcReurn t = new testExcReurn();
        System.out.println("和="+t.add(9, 34));
    }
}
/*
结果：
finally语句块
和=0
解释：finnaly中的return语句会覆盖try的return语句。
*/
```



##### 3、return的数据是引用数据类型

```java
public class testExcReturn {
    public int a;
    public testExcReturn set() {
        try {
            this.a  = 10;
            return this;
        }catch(Exception e){
            System.out.println("catch语句块");
        }finally {
            System.out.println("finally语句块");
            this.a = 200;
        }
        return this;
    }
    public static void main(String[] args) {
        testExcReturn t = new testExcReturn();
        t.set();
        System.out.println("a = "+ t.a);
    }
}
/*
结果：
finally语句块
a = 200
*/

```



##### 4、return的数据是基本数据类型

```java
public class testExcReturn {
	public int set(int a) {
		try {
			a = 10;
			return a;
		} catch (Exception e) {
			System.out.println("catch语句块");
		} finally {
			System.out.println("finally语句块");
			a = 200;
		}
        return a;
	}

	public static void main(String[] args) {
		testExcReturn t = new testExcReturn();
		System.out.println(t.set(10));
	}
}
/*
结果：
finally语句块
a = 10
*/
```



##### 结论：

1、finally覆盖catch：

​	（1）如果finally有return会覆盖catch里的throw，同样如果finally里有throw会覆盖catch里的return。

​	（2） 如果catch里和finally都有return， finally中的return会覆盖catch中的。throw也是如此。

2、catch有return而finally没有：

​	当 try 中抛出异常且catch 中有 return 语句，finally 中没有 return 语句， java 先执行 catch 中非 return 语句，再执行 finally 语句，最后执行 catch 中 return 语句。

3、try有return语句，后续还有return语句，分为以下三种情况：

​	情况一：如果finally中有return语句，则会将try中的return语句”覆盖“掉，直接执行finally中的return语句，得到返回值，这样便无法得到try之前保留好的返回值。

​	情况二：如果finally中没有return语句，也没有改变要返回值，则执行完finally中的语句后，会接着执行try中的return语句，返回之前保留的值。

​	情况三：如果finally中没有return语句，但是改变了要返回的值，这里有点**类似与引用传递和值传递的区别**，分以下两种情况，：

​		（1）如果return的数据是基本数据类型或文本字符串，则在finally中对该基本数据的改变**不起作用**，try中的return语句依然会返回进入finally块之前保留的值。

​		（2）如果return的数据是引用数据类型，而在finally中对该引用数据类型的属性值的改变**起作用**，try中的return语句返回的就是在finally中改变后的该属性的值。

<!--通常，finally 主要用于清理资源，不要把改变控制流的语句（return、throw、break、continue）放在finally 中-->



补充一些结合异常的方法的声明：

```java
	void method1() throws IOException {
	} // 合法

	// 编译错误，必须捕获或声明抛出IOException
	void method2() {
		method1();
	}

	// 合法，声明抛出IOException
	void method3() throws IOException {
		method1();
	}

	// 合法，声明抛出Exception，IOException是Exception的子类
	void method4() throws Exception {
		method1();
	}

	// 合法，捕获IOException
	void method5(){  
	 try{  
	    method1();  
	 }catch(IOException e){…}  
	}

	// 编译错误，必须捕获或声明抛出Exception
	void method6() {
		try {
			method1();
		} catch (IOException e) {
			throw new Exception();
            //编译错误是由于Exception是IOException的父类，而throw抛出的只能是捕捉到的异常类或者其子类的实例对象。
		}
	}

	// 合法，声明抛出Exception
	void method7() throws Exception {
		try {
			method1();
		} catch (IOException e) {
			throw new Exception();
		}
	}
```







