# 一、枚举

<!--类的对象只有有限个，是确定的。-->

<!--当定义一组常量的时候，建议使用枚举类。-->

<!--如果枚举类中只有一个对象，则可以作为单例模式的一种实现方式。-->

##### 1、实现：

```java
//不使用enum
class Season{
    //1.声明Season对象的属性
    private final String seasonName;
    private final String seasonDexc;

    //2.私有化构造器
    private Season(String seasonName,String seasonDexc){
        this.seasonName  = seasonName;
        this.seasonDexc = seasonDexc;
    }

    //3.提供枚举类的多个对象：public static final的
    public static final Season SPRING = new Season("春天","春暖花开");
    public static final Season SUMMER = new Season("夏天","夏日炎炎");
    public static final Season AUTUMN = new Season("秋天","秋高气爽");
    public static final Season WINTER = new Season("冬天","冰天雪地");

    //4.其他诉:1：获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDexc() {
        return seasonDexc;
    }
    //其他诉求2：提供toString()方法
    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDexc='" + seasonDexc + '\'' +
                '}';
    }
}

//使用enum：
enum Season{
    //1.提供枚举类的对象,多个对象直接用逗号","隔开，末尾对象以分号";"结束
    SPRING("春天","春暖花开"),
    SUMMER("夏天","夏日炎炎"),
    AUTUMN("秋天","秋高气爽"),
    WINTER("冬天","冰天雪地");

    //2.声明Season对象的属性
    private final String seasonName;
    private final String seasonDexc;

    //3.私有化构造器
    private Season(String seasonName,String seasonDexc){
        this.seasonName  = seasonName;
        this.seasonDexc = seasonDexc;
    }

    //4.其他诉:1：获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDexc() {
        return seasonDexc;
    }
}
```



##### 2、Enum类中的常用方法：

```java
//values()：返回枚举类型对象数组。该方法可以很方便地遍历每一个枚举值
Season[] values = Season.values();
for (int i = 0; i < values.length; i++) {
    System.out.println(values[i]);
}
//valueOf()：可以把一个字符串转为对应的枚举类对象。要求字符串必须是枚举类对象
//toString()：返回当前枚举类对象常量的名称
Season spring =Season.valueOf("SPRING");
Season spring1 =Season.valueOf("SPRING1");//报异常
System.out.println(spring.toString());
```



##### 3、使用enum关键字定义的枚举类实现接口的情况：

（1）在enum类中实现抽象方法

```java
interface Info{
    void show();
}
enum Season implements Info{
    SPRING("春天","春暖花开"),
    SUMMER("夏天","夏日炎炎"),
    AUTUMN("秋天","秋高气爽"),
    WINTER("冬天","冰天雪地");

    @Override
    public void show(){
        System.out.println("这是一个季节");
    }
}
```

（2）让枚举类的对象分别实现接口中的抽象方法

```java
public class EnumTest {
    public static void main(String[] args) {
        Season spring = Season.SPRING;
        System.out.println(spring);
        spring.show();
        Season winter = Season.WINTER;
        System.out.println(winter);
        winter.show();
    }
}

interface Info{
    void show();
}

enum Season implements Info{
    SPRING("春天","春暖花开"){
        @Override
        public void show() {
            System.out.println("我是春天");
        }
    },
    SUMMER("夏天","夏日炎炎"){
        @Override
        public void show() {
            System.out.println("我是夏天");
        }
    },
    AUTUMN("秋天","秋高气爽"){
        @Override
        public void show() {
            System.out.println("我是秋天");
        }
    },
    WINTER("冬天","冰天雪地"){
        @Override
        public void show() {
            System.out.println("我是冬天");
        }
    };
}
```



# 二、注解

定义：代码里的**特殊标记**，这些标记可以在编译，类加载，运行时被读取，并执行相应的处理。通过使用**Annotation**，程序员可以再不改变原有逻辑的情况下，在源文件中嵌入一些补充信息。

在JavaSE中，注解的使用比较简单，例如标记过时的功能、忽略警告等。在JavaEE中占据了更重要的角色，例如用来配置应用程序的任何切面，代替JavaEE旧版本中所一流的繁冗代码和XML配置等。

使用：生成文档相关的注解、在编译时进行格式检查等等

##### 1、JDK内置的三个基本注解：

```java
/*
@Oerride:限定重写父类方法,该注解只能用于方法
@Deprecated：用于表示所修饰的元素（类、方法等）已过时。通常是因为所修饰的结构危险或者存在更好的选择
@SuppressWarning：抑制编译器警告
```



##### 2、自定义注解：参照**SuppressWarning**定义

（1）注解声明为：@interface

（2）内部自定义成员，通常使用value表示

（3）可以指定成员的默认值，使用default定义

（4）如果自定义注解没有成员，表明是一个标识作用

如果注解有成员，在使用注解时，需要指明成员的值。

自定义注解必须配上注解的信息处理流程（使用反射）才有意义

```java
public @interface myAnnotation {

    String value() default "hello";
}
@myAnnotation(value = "hi")
class Person{
    
}
```

##### 3、JDK提供的四种元注解

定义：对现有的注解进行解释说明的注解

（1）**Retention：**指定所修饰**Annotation**的生命周期：**SOURCE\CLASS（默认行为）\RUNTIME**，只有声明为RUNTIME生命周期的注解，才能通过反射获取。

（2）**Target：**用于指定被修饰的**Annotation**能用于修饰哪些程序元素

（3）**Documented：**表示所修饰的注解在被javadoc解析时，保留下来

（4）**Inherited：**被它修饰的Annotation将具有继承性

##### 4、新特性：

（1）可重复注解：

（2）类型注解：









