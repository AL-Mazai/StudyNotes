# 常用类：

#### 一：String类

**String:字符串，使用一对" "引起来表示**

​    1、String声明为final，不可被继承

​    2、String实现了Serializable接口：表示字符串是支持序列化的

​		          实现了Comparable接口：表示String可以比较大小

​    3、String内部定义了final char[] value用于存储字符串数据

##### （一）不可变性：

**String：**代表不可变的字符序列。即不可变性

1、当对字符串重新赋值的时候，需要重写指定内存区域赋值，不能使用原有的value进行赋值

2、当对现有的字符串进行连接操作时，也需要重写指定内存区域赋值，不能使用原有的value进行赋值

3、当调用String的replace()方法修改指定字符串时，也需要重写指定内存区域赋值，不能使用原有的value进行赋值

```java
public class StringTest {
    @Test
    public void test1(){
        String s1 = "abc";
        String s2 = "abc";

        System.out.println(s1 == s2);//true
        s1 = "hello";

        System.out.println(s1);//hello
        System.out.println(s2);//abc

        System.out.println("****************************");

        String s3 = "abc";
        s3 += "def";
        System.out.println(s3);//abcdef
        System.out.println(s2);//abc

        System.out.println("****************************");

        String s4 = "abc";
        String s5 = s4.replace('a', 'm');
        System.out.println(s4);//abc
        System.out.println(s5);//mbc

    }
}
```



##### （二）实例化方式：

```java
 public void test2(){
        //通过字面量定义的方式：此时的s1和s2的数据JavaEE声明在方法区中的字符串常量池中
        String s1 = "javaEE";
        String s2 = "javaEE";
        //通过new + 构造器方式：此时的s3和s4保存的地址值，是数据在堆空间中开辟空间以后对应的地址值
        String s3 = new String("javaEE");
        String s4 = new String("javaEE");

        System.out.println(s1 == s2);//true
        System.out.println(s1 == s3);//false
        System.out.println(s1 == s4);//false
        System.out.println(s3 == s4);//false
    }
```

![image-20220326161837184](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220326161837184.png)

**字符串拼接补充：**

1、**常量与常量**的拼接结果在常量池，且常量池中不会存在相同的内容的常量

<!--注：用final修饰的也是常量-->

2、只要其中有一个是变量，结果就在堆中

3、如果拼接的结果调用inter() 方法，返回值就在常量池中

```java
public void test3(){
        String s1 = "javaEE";
        String s2 = "hadoop";

        String s3 = "javaEEhadoop";//方法区字符串常量池
        String s4 = "javaEE" + "hadoop";//方法区字符串常量池
    
        String s5 = s1 + "javaEE";//堆
        String s6 = "javaEE" + s2;//堆
        String s7 = s1 + s2;//堆
        
        System.out.println(s3 ==s4);//true
        System.out.println(s3 ==s5);//false
        System.out.println(s3 ==s6);//false
        System.out.println(s5 ==s6);//false
        System.out.println(s5 == s7);//false
        System.out.println(s6 == s7);//false
    
        String s8 = s5.intern();//返回值得到的s8使用的常量值中已经存在“JavaEEHadoop”
        System.out.println(s3 == s8);//ture
    }
```



##### （三）常用方法：

```java
public class StringMethodTest {
    @Test
    public void test1(){
        String s1 = "HELLOworld";
        System.out.println(s1.length());
        System.out.println(s1.charAt(0));//取字符串下标为index的字符
        System.out.println(s1.charAt(9));
        //System.out.println(s1.charAt(10));越界

        System.out.println(s1.isEmpty());//判断是否空字符

        String s2 = s1.toLowerCase();//转小写
        System.out.println(s2);
        String s3 = s1.toUpperCase();
        System.out.println(s3);//转大写
        System.out.println(s1);//s1不会改变

        String s4 = "   he  llo   world   ";
        String s5 = s4.trim();//去除首位空格
        System.out.println("-----" + s4 + "-----");
        System.out.println("-----" + s5 + "-----");

        String s6 = "HELLOworld";
        String s7 = "helloworld";
        System.out.println(s6.equals(s7));
        System.out.println(s6.equalsIgnoreCase(s7));//与equals类似，比较时忽略大小写

        String s8 = s1.concat("");//拼接字符串：返回值在堆中
        System.out.println(s8);

        String s9 = "abc";
        String s10 = "abf";
        System.out.println(s9.compareTo(s10));//比较字符串大小,设计字符串的排序

        String s11 = "北京尚硅谷教育";
        String s12 = s11.substring(2);//取子串：从下标为2的字符开始取
        System.out.println(s12);
        String s13 = s11.substring(2,5);//取子串：从下标为2的字符开始取，取到4，相当于左闭右开区间
        System.out.println(s12);
    }

}
```

```java
public void test2(){
        String str1 = "helloworld";
        boolean b1 = str1.endsWith("ld");//检测是否以指定的后缀结束
        System.out.println(b1);//true

        boolean b2 = str1.startsWith("he");//检测是否以指定的前缀开始
        System.out.println(b2);//true
        boolean b3 = str1.startsWith("ll",2);//检测字符串的子串是否以指定的前缀开始
        System.out.println(b3);//false

        String str2 = "wo";
        System.out.println(str1.contains(str2));//判断字符串是否包含指定的子串
    
        System.out.println(str1.indexOf("lo"));//以从头搜索的方式，返回指定子字符串在此字符串中第一次出现的处的索引
        System.out.println(str1.indexOf("lo",5));//以指定的索引开始从头搜索的方式，返回指定子字符串在此字符串中第一次出现的处的索引

        String str3 = "hellorworld";
        System.out.println(str3.lastIndexOf("or"));//以反向搜索的方式，返回指定子字符串在此字符串中最后一次出现处的索引
        System.out.println(str3.lastIndexOf("or",3));//以指定的索引开始反向搜索的方式，返回指定子字符串在此字符串中最后一次出现的处的索引，
    }
```



##### （四）String类型转化为其他数据类型：

<!--解码时，要求解码使用的字符集必须和编码时使用的字符集一致，否则会出现乱码-->

```java
public void test2() throws UnsupportedEncodingException {
    String str1 = "abcd1239？?中国";

    //String 与 char 数组之间的转化
    char[] chars = str1.toCharArray();//使用toCharArray方法
    System.out.println(chars);//使用String的构造器
    String s = new String(chars);

    //String 与 byte 数组之间的转化
    byte[] bytes = str1.getBytes();//使用默认的字符集进行转换（这里是UTF-8）
    System.out.println(Arrays.toString(bytes));

    byte[] gbks = str1.getBytes("gbk");//使用gbk字符集进行编码
    System.out.println(Arrays.toString(gbks));

    System.out.println("************************");
    String str2 = new String(bytes);//使用默认字符集进行解码（这里是UTF-8）
    System.out.println(str2);

    String str3 = new String(gbks);
    System.out.println(str3);//出现乱码：编码集和解码集不一样

    String str4 = new String(gbks, "gbk");//使用gbk字符集进行解码,所以str4不是乱码
    System.out.println(str4);
}
```



##### （五）StringBuffer 和 StringBuilder

**String：**不可变的字符序列

**StringBuffer：**可变的字符序列；线程安全的，效率低

**StringBuilder：**可变的字符序列；线程不安全，效率高

三者效率：StringBuilder>StringBuffer>String

```java
public void test1(){
    StringBuffer sb1 = new StringBuffer("abc");
    sb1.setCharAt(0,'m');
    System.out.println(sb1);
}
```

分析：

```java
String str = new String()；//new char[0]

String str = new String("abc")；//new char[]{'a','b','c'}

StringBuffer sb1 = new StringBuffer();//char[] value = new char[16];底层创建一个长度是16的数组
System.out.println(sb1.length());//0
StringBuffer sb2 = new StringBuffer("abc");//char[] value = new char["abc".length() + 16]
System.out.println(sb2.length());//3
//扩容问题：如果要添加的数据底层数组放不下，那就需要扩容底层的数组，默认情况下，扩容为原来的2倍+2，同时将原有数组的元素复制到新的数组中
//意义：开发中建议使用StringBuffer或者StringBuilder的带参数的构造器
```

StringBuffer类的常用方法：

```java
/*
增：append(xxx)

删：delete(int start,int end)

改：setChar(int n , char ch)

查：charAt(int n)

插：insert(int offset,xxx)

长度：length()

遍历：for() + charAt() 或者 toString()
```



#### 二、Date类：

`java.util`包提供了`Date`类来封装当前的日期和时间。`Date`类提供两个构造函数来实例化`Date`对象。

第一个构造函数使用当前日期和时间来初始化对象。

```java、
Date( )  
```

第二个构造函数接收一个参数，该参数是从1970年1月1日起的毫秒数。

```java
Date(long millisec)  
```

Date对象创建以后，可以调用下面的方法。

![img](https://data.educoder.net/api/attachments/207182)  

![img](https://data.educoder.net/api/attachments/207183)  



##### 获取当前日期时间：

Java中获取当前日期和时间很简单，使用`Date`对象的`toString()`方法来打印当前日期和时间：

```java
import java.util.Date;
public class DateDemo {  
    public static void main(String args[]) {  
        // 初始化 Date 对象  
        Date date = new Date();
        // 使用 toString() 函数显示日期时间  
        System.out.println(date.toString());  
    }  
}
```

输出结果：
`Sun Aug 12 23:33:44 CST 2018`

##### 日期比较：

Java使用以下三种方法来比较两个日期：

- 使用`getTime()`方法获取两个日期自1970年1月1日经历的毫秒数值，然后比较这两个值；
- 使用方法`before()`，`after()`和`equals()`。例如，一个月的12号比18号早，则 `new Date(99, 2, 12).before(new Date (99, 2, 18))`返回`true`；
- 使用`compareTo()`方法，它是由`Comparable`接口定义的，`Date` 类实现了这个接口。

##### 使用 SimpleDateFormat 格式化日期

`SimpleDateFormat`是一个以语言环境敏感的方式来格式化和分析日期的类。`SimpleDateFormat`允许你选择任何用户自定义日期时间格式来运行。例如：

```java
import java.util.*;  
import java.text.*;
public class DateDemo {  
    public static void main(String args[]) {
        Date dNow = new Date();  
        SimpleDateFormat ft = new SimpleDateFormat( "E yyyy.MM.dd 'at' hh:mm:ss a zzz");
        System.out.println("Current Date: " ft.format(dNow));  
    }  
}
```



**SimpleDateFormat ft = new SimpleDateFormat ("E yyyy.MM.dd 'at' hh:mm:ss a zzz");**  这一行代码确立了转换的格式，其中`yyyy`是完整的公元年，`MM`是月份，`dd`是日期，`hh`:`mm`:`ss` 是时、分、秒。

注意:有的格式大写，有的格式小写，例如`MM`是月份，`mm`是分；`HH` 是 24 小时制，而 hh 是 12 小时制。

输出结果如下:
`Current Date: 星期日 2018.08.12 at 11:45:16 下午 CST`

##### 日期和时间的格式化编码：

时间模式字符串用来指定时间格式。在此模式中，所有的`ASCII`字母被保留为模式字母，定义如下：

![img](https://data.educoder.net/api/attachments/207184)  

![img](https://data.educoder.net/api/attachments/207185)  

使用`printf`格式化日期

`printf`方法可以很轻松地格式化时间和日期。使用两个字母格式，它以`%t`开头并且以下面表格中的一个字母结尾。

![img](https://data.educoder.net/api/attachments/207186)  

实例：

```java
import java.util.Date;
public class DateDemo {
    public static void main(String args[]) {  
        // 初始化 Date 对象  
        Date date = new Date();
        // c的使用  
        System.out.printf("全部日期和时间信息：%tc%n", date);  
        // f的使用  
        System.out.printf("年-月-日格式：%tF%n", date);  
        // d的使用  
        System.out.printf("月/日/年格式：%tD%n", date);  
        // r的使用  
        System.out.printf("HH:MM:SS PM格式（12时制）：%tr%n", date);  
        // t的使用  
        System.out.printf("HH:MM:SS格式（24时制）：%tT%n", date);  
        // R的使用  
        System.out.printf("HH:MM格式（24时制）：%tR", date);  
    }  
} 
```

输出结果：

```java
全部日期和时间信息：星期日 八月 12 23:51:03 CST 2018
`年-月-日格式：2018-08-12`
`月/日/年格式：08/12/18`
`HH`:`MM`:`SS` `PM格式（12时制）：11:51:03 下午`
`HH`:`MM`:`SS` `格式（24时制）：23:51:03`
`HH`:`MM` `格式（24时制）：23:51`
```

如果需要重复提供日期，那么利用这种方式来格式化它的每一部分就有点复杂了。因此，可以利用一个格式化字符串指出要被格式化的参数的索引。

索引必须紧跟在`%`后面，而且必须以`$`结束。例如：

```java
import java.util.Date;
public class DateDemo {
    public static void main(String args[]) {  
        // 初始化 Date 对象  
        Date date = new Date();
        // 使用toString()显示日期和时间  
        System.out.printf("%1$s %2$tB %2$td, %2$tY", "Due date:", date);  
    }  
} 
```

输出结果：
`Due date: 八月 12, 2018`

或者，可以使用`<`标志。它表明先前被格式化的参数要被再次使用。例如：

```java
import java.util.Date;
public class DateDemo {
    public static void main(String args[]) {  
        // 初始化 Date 对象  
        Date date = new Date();
        // 显示格式化时间  
        System.out.printf("%s %tB %<te, %<tY", "Due date:", date);  
    }  
} 
```

输出结果：
`Due date: 八月 12, 2018`

定义日期格式的转换符可以使日期通过指定的转换符生成新字符串。这些日期转换符如下所示：

```java
public class DateDemo {  
    public static void main(String args[]) {  
        Date date = new Date();  
        // b的使用，月份简称  
        String str = String.format(Locale.US, "英文月份简称：%tb", date);  
        System.out.println(str);  
        System.out.printf("本地月份简称：%tb%n", date);  
        // B的使用，月份全称  
        str = String.format(Locale.US, "英文月份全称：%tB", date);  
        System.out.println(str);  
        System.out.printf("本地月份全称：%tB%n", date);  
        // a的使用，星期简称  
        str = String.format(Locale.US, "英文星期的简称：%ta", date);  
        System.out.println(str);  
        // A的使用，星期全称  
        System.out.printf("本地星期的简称：%tA%n", date);  
        // C的使用，年前两位  
        System.out.printf("年的前两位数字（不足两位前面补0）：%tC%n", date);  
        // y的使用，年后两位  
        System.out.printf("年的后两位数字（不足两位前面补0）：%ty%n", date);  
        // j的使用，一年的天数  
        System.out.printf("一年中的天数（即年的第几天）：%tj%n", date);  
        // m的使用，月份  
        System.out.printf("两位数字的月份（不足两位前面补0）：%tm%n", date);  
        // d的使用，日（二位，不够补零）  
        System.out.printf("两位数字的日（不足两位前面补0）：%td%n", date);  
        // e的使用，日（一位不补零）  
        System.out.printf("月份的日（前面不补0）：%te", date);  
    }  
}
```

输出结果：
`英文月份简称：Aug`
`本地月份简称：八月`
`英文月份全称：August`
`本地月份全称：八月`
`英文星期的简称：Sun`
`本地星期的简称：星期日`
`年的前两位数字（不足两位前面补0）：20`
`年的后两位数字（不足两位前面补0）：18`
`一年中的天数（即年的第几天）：224`
`两位数字的月份（不足两位前面补0）：08`
`两位数字的日（不足两位前面补0）：12`
`月份的日（前面不补0）：12`

##### 解析字符串为时间：

`SimpleDateFormat`类有一些附加的方法，特别是`parse()`，它试图按照给定的`SimpleDateFormat`对象的格式化存储来解析字符串。例如：

```java
import java.text.ParseException;  
import java.text.SimpleDateFormat;  
import java.util.Date;
public class DateDemo {
    public static void main(String args[]) {  
        SimpleDateFormat ft = new SimpleDateFormat("yyyy-MM-dd");
        String input = args.length == 0 ? "1818-11-11" : args[0];
        System.out.print(input + " Parses as ");
        Date t;
        try {  
            t = ft.parse(input);  
            System.out.println(t);  
        } catch (ParseException e) {  
            System.out.println("Unparseable using " + ft);  
        }  
    }  
}  
```

输出结果：
`1818-11-11 Parses as Wed Nov 11 00:00:00 CST 1818`

**Java休眠(`sleep`)**

`sleep()`使当前线程进入停滞状态（阻塞当前线程），让出`CPU`的使用、目的是不让当前线程独自霸占该进程所获的`CPU`资源，以留一定时间给其他线程执行的机会。

可以让程序休眠一毫秒的时间或者到您的计算机的寿命长的任意段时间。例如，下面的程序会休眠3秒：

```java
import java.util.*;
public class SleepDemo {  
    public static void main(String args[]) {  
        try {  
            System.out.println(new Date() + "\n");  
            Thread.sleep(1000 * 3); // 休眠3秒  
            System.out.println(new Date() + "\n");  
        } catch (Exception e) {  
            System.out.println("Got an exception!");  
        }  
    }  
}  
```

输出结果：
`Mon Aug 13 00:05:33 CST 2018`

```
Mon Aug 13 00:05:36 CST 2018
```

##### 测量时间：

下面的一个例子表明如何测量时间间隔（以毫秒为单位）：

```java
import java.util.Date;
public class DiffDemo {
    public static void main(String args[]) {  
        try {  
            long start = System.currentTimeMillis();  
            System.out.println(new Date() + "\n");  
            Thread.sleep(5 * 60 * 10);  
            System.out.println(new Date() + "\n");  
            long end = System.currentTimeMillis();  
            long diff = end - start;  
            System.out.println("Difference is : " + diff);  
        } catch (Exception e) {  
            System.out.println("Got an exception!");  
        }  
    }  
}  
```

输出结果：
`Mon Aug 13 00:07:58 CST 2018`

```
Mon Aug 13 00:08:01 CST 2018
Difference is : 3021
```



#### 三、random类

##### （一）Random类

`Random`类位于`java.util`包下，`Random`类中实现的随机算法是伪随机，也就是有规则的随机。在进行随机时，随机算法的起源数字称为种子数(`seed`)，在种子数的基础上进行一定的变换，从而产生需要的随机数字。

相同种子数的`Random`对象，相同次数生成的随机数字是完全相同的。也就是说，两个种子数相同的`Random`对象，第一次生成的随机数字完全相同，第二次生成的随机数字也完全相同。这点在生成多个随机数字时需要特别注意。

##### （二）Random对象的生成

`Random`类包含两个构造方法，下面依次进行介绍：

```java
public Random()  
```

该构造方法使用一个和当前系统时间对应的相对时间有关的数字作为种子数，然后使用这个种子数构造`Random`对象。

```java
public Random(long seed)  
```

该构造方法可以通过制定一个种子数进行创建。

示例：

```java
Random r = new Random();
Random r1 = new Random(10);  
```

<!--种子数只是随机算法的起源数字，和生成的随机数字的区间无关。-->

验证：相同种子数的`Random`对象，相同次数生成的随机数字是完全相同的。

```java
import java.util.Random;
public class RandomTest {  
    public void random() {  
        int i = 0;  
        int j = 0;  
        Random random = new Random(1);  
        Random random1 = new Random(1);  
        i = random.nextInt();  
        j = random1.nextInt();  
        System.out.println("i:" + i + "\nj:" + j);  
    }
    public static void main(String[] args) {  
        RandomTest tt = new RandomTest();  
        tt.random();  
    }
} 
```

输出结果：
第一次：
`i:-1155869325`
`j:-1155869325`

修改一下起源数字，让其等于`100`。

```
Random random = new Random(100);  Random random1 = new Random(100);  
```

输出结果：
`i:-1193959466`
`j:-1193959466`

##### （三）Random类中的常用方法

`Random`类中各方法生成的随机数字都是**均匀分布**的，也就是说区间内部的数字生成的几率是均等的。下面对这些方法做一下基本的介绍：

```java
import java.util.Random;
public class RandomTest {  
    public static void main(String[] args) {  
        Random random = new Random();  
        System.out.println("nextInt()：" + random.nextInt()); // 随机生成一个整数，这个整数的范围就是int类型的范围-2^31~2^31-1  
        System.out.println("nextLong()：" + random.nextLong()); // 随机生成long类型范围的整数  
        System.out.println("nextFloat()：" + random.nextFloat()); // 随机生成[0, 1.0)区间的小数  
        System.out.println("nextDouble()：" + random.nextDouble()); // 随机生成[0, 1.0)区间的小数  
        System.out.println("nextBoolean()："+random.nextBoolean());//随机生成一个boolean值，生成true和false的值几率相等，也就是都是50%的几率  
        System.out.println("nextGaussian():"+random.nextGaussian());//随机生成呈高斯（“正态”）分布的 double 值，其平均值是 0.0，标准差是 1.0  
        byte[] byteArr = new byte[5];  
        random.nextBytes(byteArr); // 随机生成byte，并存放在定义的数组中，生成的个数等于定义的数组的个数  
        System.out.print("nextBytes()：");  
        for (int i = 0; i < byteArr.length; i++) {  
                System.out.print(byteArr[i]+"\t");  
        }  
        System.out.println();
        /**  
         * random.nextInt(n)   
         * 随机生成一个正整数，整数范围[0,n),包含0而不包含n  
         * 如果想生成其他范围的数据，可以在此基础上进行加减  
         *  
         * 例如：   
         * 1. 想生成范围在[0,n]的整数   
         *         random.nextInt(n+1)   
         * 2. 想生成范围在[m,n]的整数, n > m  
         *         random.nextInt(n-m+1) + m   
         *         random.nextInt() % (n-m) + m   
         * 3. 想生成范围在(m,n)的整数  
         *         random.nextInt(n-m+1) + m -1   
         *         random.nextInt() % (n-m) + m - 1   
         * ......主要是依靠简单的加减法  
         */  
        System.out.println("nextInt(10)：" + random.nextInt(10)); // 随机生成一个整数，整数范围[0,10)  
        for (int i = 0; i < 5; i++) {  
            System.out.println("我生成了一个[3,15)区间的数，它是：" + (random.nextInt(12) + 3));  
        }  
        /**  
         * random.nextDouble()   
         * 例如:  
         * 1.生成[0,1.0)区间的小数  
         *         double d1 = random.nextDouble();//直接使用nextDouble方法获得。  
         * 2.生成[0,5.0)区间的小数  
         *           double d2 = random.nextDouble() * 5;//因为扩大5倍即是要求的区间。同理，生成[0,d)区间的随机小数，d为任意正的小数，则只需要将nextDouble方法的返回值乘以d即可。  
         * 3.生成[1,2.5)区间的小数  
         *         double d3 = r.nextDouble() * 1.5 + 1;//生成[1,2.5)区间的随机小数，则只需要首先生成[0,1.5)区间的随机数字，然后将生成的随机数区间加1即可。  
         * ......同理，生成任意非从0开始的小数区间[d1,d2)范围的随机数字(其中d1不等于0)，则只需要首先生成[0,d2-d1)区间的随机数字，然后将生成的随机数字区间加上d1即可。  
         *   
         */  
        }  
}  
```

输出结果：
`nextInt()：1842341002`
`nextLong()：4006643082448092921`
`nextFloat()：0.88948154`
`nextDouble()：0.5635189241159165`
`nextBoolean()：false`
`nextGaussian():1.3191426544832998`
`nextBytes()：36    100    94    14    -98    `
`nextInt(10)：1`
`我生成了一个[3,15)区间的数，它是：5`
`我生成了一个[3,15)区间的数，它是：10`
`我生成了一个[3,15)区间的数，它是：10`
`我生成了一个[3,15)区间的数，它是：11`
`我生成了一个[3,15)区间的数，它是：6`

JDK1.8新增方法：

```java
import java.util.Random;
public class RandomTest2 {  
    /**  
     * 测试Random类中 JDK1.8提供的新方法 JDK1.8新增了Stream的概念 在Random中，为double, int,  
     * long类型分别增加了对应的生成随机数的方法 鉴于每种数据类型方法原理是一样的，所以，这里以int类型举例说明用法  
     */
    public static void main(String[] args) {  
        Random random = new Random();  
        random.ints(); // 生成无限个int类型范围内的数据，因为是无限个，这里就不打印了，会卡死的......  
        random.ints(10, 100); // 生成无限个[10,100)范围内的数据
        /**  
         * 这里的toArray 是Stream里提供的方法  
         */  
        int[] arr = random.ints(5).toArray(); // 生成5个int范围类的整数。  
        System.out.println(arr.length);  
        for (int i = 0; i < arr.length; i++) {  
            System.out.println(arr[i]);  
        }
        // 生成5个在[10,100)范围内的整数  
        arr = random.ints(5, 10, 100).toArray();  
        for (int i = 0; i < arr.length; i++) {  
            System.out.println(arr[i]);  
        }
        /**  
         * 对于 random.ints(); random.ints(ori, des);  
         * 两个生成无限个随机数的方法，我们可以利用Stream里的terminal操作，来截断无限这个操作  
         */  
        // limit表示限制只要5个，等价于random.ints(5)  
        arr = random.ints().limit(5).toArray();  
        System.out.println(arr.length);  
        for (int i = 0; i < arr.length; i++) {  
            System.out.println(arr[i]);  
        }
        // 等价于random.ints(5, 10, 100)  
        arr = random.ints(10, 100).limit(5).toArray();  
        for (int i = 0; i < arr.length; i++) {  
            System.out.println(arr[i]);  
        }  
    }
}  
```

输出结果：
`5`
`1801462452`
`-1812435985`
`-1073912930`
`1160255210`
`-1342018704`
`80`
`54`
`16`
`67`
`82`
`5`
`-1161610558`
`283052091`
`797550518`
`-275356995`
`-1661722790`
`11`
`27`
`27`
`52`
`54`



#### 四、Math 类

`Math`类是一个工具类，它的构造器被定义成`private`的，因此无法创造`Math`类的对象。`Math`类中所有的方法都是类方法，可以直接通过类名来调用他们。

`Math`类包含完成基本数学函数所需的方法。这些方法基本可以分为三类：**三角函数方法、指数函数方法和服务方法。**在`Math`类中定义了`PI`和`E`两个`double`型常量，`PI`就是`π`的值，而`E`即`e`指数底的值，分别是：`3.141592653589793`和`2.718281828459045`。

##### Math类中常用方法

###### 三角函数方法

`Math`类包含下面的三角函数方法：

- ` Math.toDegrees`  :将-π/2到π/2之间的弧度值转化为度

  ```java
  Math.toDegrees(Math.PI/2)
  //结果为90.0；
  ```

- `Math.toRadians`  :将度转化为`-π/2`到`π/2`之间的弧度值，例如：

  ```java
  Math.toRadians(30)
  //结果为`π/6`；
  ```

- `Math.sin`、`Math.cos`、`Math.tan`这三个方法是三角函数中的正弦、余弦和正切，反之`Math.asin`、`Math.acos`、`Math.atan`是他们的反函数。

###### 指数函数方法

`Math`类中有五个与指数函数相关的方法`Math.exp(a)`方法主要是获得以 e 为底 a 为指数的数值；`Math.log()`和`Math.log10()`是对数函数；`Math.pow(a,b)`是以a为底b为指数的值；`Math.sqrt() `是开根号。

- 取整方法；

`Math`类里包含五个取整方法：`Math.ceil()`方法是往大里取值；`Math.floor()`方法是往小里取值；`Math.rint()`方法返回与参数最接近的整数，返回类型为`double`，注意`.5`的时候会取偶数；`Math.round()`方法分两种：`int`型和`long`型，`Math.round(a)`就是`Math.floor(a+0.5)`，即将原来的数字加上`0.5`后再向下取整，所以，`Math.round(11.5)`的结果为`12`，`Math.round(-11.5)`的结果为`-11`。

- ` min`、`max`和`abs`方法

取最大值和最小值以及绝对值。

- ` random`方法。

生成随机数取值范围是`0.0`到`1.0`的`double`型数值。也可以用简单的表达式生成任意范围的随机数，例如：
`(int）(Math.random()*10) `返回`0`到`9`之间的一个随机整数。



代码示例：

```java
public class MathTest{  
      public static void main(String args[]){   
        /**   
         *Math.sqrt()//计算平方根  
         *Math.cbrt()//计算立方根  
         *Math.pow(a, b)//计算a的b次方  
         *Math.max( , );//计算最大值  
         *Math.min( , );//计算最小值  
         */  
        System.out.println(Math.sqrt(16));  //4.0   
        System.out.println(Math.cbrt(8));  //2.0  
        System.out.println(Math.pow(3,2));   //9.0  
        System.out.println(Math.max(2.3,4.5));//4.5  
        System.out.println(Math.min(2.3,4.5));//2.3  
        /**   
         * abs求绝对值   
         */  
        System.out.println(Math.abs(-10.4));  //10.4   
        System.out.println(Math.abs(10.1));   //10.1   
        /**   
         * ceil天花板的意思，就是返回大的值  
         */  
        System.out.println(Math.ceil(-10.1));  //-10.0   
        System.out.println(Math.ceil(10.7));  //11.0   
        System.out.println(Math.ceil(-0.7));  //-0.0   
        System.out.println(Math.ceil(0.0));   //0.0   
        System.out.println(Math.ceil(-0.0));  //-0.0   
        System.out.println(Math.ceil(-1.7));  //-1.0  
        /**   
         * floor地板的意思，就是返回小的值   
         */  
        System.out.println(Math.floor(-10.1)); //-11.0   
        System.out.println(Math.floor(10.7));  //10.0   
        System.out.println(Math.floor(-0.7));  //-1.0   
        System.out.println(Math.floor(0.0));  //0.0   
        System.out.println(Math.floor(-0.0));  //-0.0   
        /**   
         * random 取得一个大于或者等于0.0小于不等于1.0的随机数   
         */  
        System.out.println(Math.random()); //小于1大于0的double类型的数  
        System.out.println(Math.random()*2);//大于0小于1的double类型的数  
        System.out.println(Math.random()*2+1);//大于1小于2的double类型的数  
        /**   
         * rint 四舍五入，返回double值   
         * 注意.5的时候会取偶数    
         */  
        System.out.println(Math.rint(10.1));  //10.0   
        System.out.println(Math.rint(10.7));  //11.0   
        System.out.println(Math.rint(11.5));  //12.0   
        System.out.println(Math.rint(10.5));  //10.0   
        System.out.println(Math.rint(10.51));  //11.0   
        System.out.println(Math.rint(-10.5));  //-10.0   
        System.out.println(Math.rint(-11.5));  //-12.0   
        System.out.println(Math.rint(-10.51)); //-11.0   
        System.out.println(Math.rint(-10.6));  //-11.0   
        System.out.println(Math.rint(-10.2));  //-10.0   
        /**   
         * round 四舍五入，float时返回int值，double时返回long值   
         */  
        System.out.println(Math.round(10.1));  //10   
        System.out.println(Math.round(10.7));  //11   
        System.out.println(Math.round(10.5));  //11   
        System.out.println(Math.round(10.51)); //11   
        System.out.println(Math.round(-10.5)); //-10   
        System.out.println(Math.round(-10.51)); //-11   
        System.out.println(Math.round(-10.6)); //-11   
        System.out.println(Math.round(-10.2)); //-10   
      }   
    }  
```

输出结果：
`4.0`
`2.0`
`9.0`
`4.5`
`2.3`
`10.4`
`10.1`
`-10.0`
`11.0`
`-0.0`
`0.0`
`-0.0`
`-1.0`
`-11.0`
`10.0`
`-1.0`
`0.0`
`-0.0`
`0.6148102966002477`
`0.026175344383210897`
`2.2298232345361297`
`10.0`
`11.0`
`12.0`
`10.0`
`11.0`
`-10.0`
`-12.0`
`-11.0`
`-11.0`
`-10.0`
`10`
`11`
`11`
`11`
`-10`
`-11`
`-11`
`-10`
