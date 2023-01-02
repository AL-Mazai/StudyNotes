# 泛型

JDK5中引入的特性，它提供了**编译时类型安全检测机制**，该机制允许在编译时检测到非法的类型，它的本质是**参数化类型**，也就是说所操作的数据类型被指定为一个参数。

一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？顾名思义，就是**将类型由原来的具体的类型参数化，然后在使用或者调用时传入具体的类型**。这种参数类型可以用在类、方法和接口中，分别被称为泛型类、泛型方法、泛型接口

**泛型定义格式:**

<类型>：指定一种类型的格式。这里的类型可以看成是形参

<类型1类型2>：指定多种类型的格式，多种类型之间用逗号隔开。这里的类型可以看成是形参

将来具体调用时候给定的类型可以看成是实参，并目实参的类型只能是引用数据类型。

**泛型的好处:**

（1）把运行时期的问题提前到了编译期间

（2）避免了强制类型转换

```java
//例：
public void test2(){
    Map<String,Integer> map = new HashMap<String,Integer>();

    map.put("tom",12);
    map.put("jie",90);
    map.put("zzw",34);
    map.put("bob",99);

    Set<Map.Entry<String,Integer>> entry = map.entrySet();
    Iterator<Map.Entry<String,Integer>> iterator = entry.iterator();

    while(iterator.hasNext()){
        Map.Entry<String, Integer> e = iterator.next();
        System.out.println(e.getKey() + "-->" + e.getValue());
    }
}
```

<!--实例化时没有用泛型，则默认为Object类型-->

##### 泛型类：

泛型类型用于类的定义中，被称为泛型类。通过泛型可以完成对一组类的操作对外开放相同的接口。最典型的就是各种容器类，如：List、Set、Map。

<!--静态方法中不能使用泛型类-->

<!--异常类也不能是异常类-->

```java
public class 类名<泛型类型> {}
```

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，建议指定T的具体类型
public class Generic<T>{ 
    //key这个成员变量的类型为T,T的类型由外部指定  
    private T key;

    public Generic(T key) { //泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }

    public T getKey(){ //泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
    /*
    public static void show(){
        System.out.println(key);
    }静态方法中不能使用泛型类，静态方法在类的加载时就存在，但是泛型类是在实例化时指明
}
```

**子类继承父类时：**

![image-20220403155656336](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220403155656336.png)

##### 泛型接口：

```java
public interface 接口名<泛型类型> {}
```

<!--格式和泛型类差不多-->

```java
interface f <T> {
    void show();
}
//实现类类型不确定
class t1 <T> implements f <T>{
    @Override
    public void show() {

    }
}
//实现类确定了类型
class t2 implements f <Integer>{
    @Override
    public void show() {

    }
}
//默认是Object类,即等价于class t3 implements f<Object>{}
class t3 implements f {
    @Override
    public void show() {

    }
}
```

##### 泛型方法：

在方法中出现了泛型结构，泛型参数与类的泛型参数没有关系。

<!--可以声明为静态的-->

```java
public <泛型类型> 返回值类型 方法名<泛型类型 变量名> {}
```



```java
class Order<T>{
    //泛型方法
    public <E> List<E> copy(E[] arr){
        ArrayList<E> list =  new ArrayList<>();
        for (E e : arr) {
            list.add(e);
        }
        return list;
    }
}

public class OrderTest {
    @Test
    public void test(){
        Order<String> order = new Order<>();
        Integer[] arr = new Integer[]{1,2,3,4,5};
        //泛型方法在调用时指明泛型参数类型
        List<Integer> list = order.copy(arr);
        System.out.println(list);
    }
}
```

补充：

<!--类A是类B的父类，但是List<A>和List<B>没有子父类关系，是并列关系，二者共同父类List<?>-->

<!--类A是类B的父类，则A<T>是B<T>的父类-->

```java
public void test3(){
    List<String> list1 = null;
    List<Object> list2 = null;
    //此时的list1和list2不具有子父类的的关系
    //list2 = list1;编译不通过
    
    List<String> list3 = null;
    ArrayList<String> list4 = null;
    list3 = list4;//可以
}
```

![image-20220403164213417](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220403164213417.png)

##### 类型通配符：

**为了表示各种LIst泛型的父类，可以使用类型通配符**

类型通配符：**<?>**（任意类型）

**List<?>：**表示元素类型未知的List，它的元素可以匹配任何的类型，这种**带通配符的List仅表示它是各种发型List的父类，并不能把元素添加到其中。**

如果不希望List<?>是任何泛型List的父类，只希望它代表某一类泛型List的父类，可以使用类型通配符的上限

类型通配符上限：**<?extend 类型>**

```java
List<? extends Number>//他表示的类型是Number或者其子类
```

类型通配符下限：**<?extend 类型>**

```java
List<? super Number>//他表示的类型是Number或者其父类
```



```java
public static void main(String[] args) {
       //<?>（任意类型）
        List<?> list1 = new List<Object>();
        List<?> lsit2 = new List<Number>();
        List<?> list3 = new List<Integer>();

    	//<? extends Number>
        //List<?extends Number> list4 = new List<Object>();错误：Object是其父类
        List<? extends Number> list5 = new ArrayList<Number>();
        List<? extends Number> list6 = new ArrayList<Integer>();

    	//<? super Number>
    	//List<? super Number> list9 = new List<Integer>();错误：Integer是其子类
        List<? super Number> list7 = new List<Object>();
        List<? super Number> list8 = new List<Number>();
    }
```

