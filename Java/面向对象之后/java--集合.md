# 集合：

特点：提供一种存储空间可变的存储模型，存储的数据容量可以发生改变，解决数组存储数据方面的弊端。

此时的存储，主要指的是内存层面的存储，不涉及到持久化的存储（.txt、.jpg、数据库中）

#### 集合框架：

- **Collection**接口：单列集合，用来存储一个一个的对象
  - **List**接口：存储有序的、可重复的数据 --》动态数组
  - **Set**接口：存储无序的、不可重复的数据 --》高中讲的“集合”
- **Map接口**：双列集合，用来存储一对（key - value）一对的数据 --》高中函数y=f(x)

### 一、Collection接口

**单列集合，用来存储一个一个的对象**

<!--向Collection接口中添加数据obj时，要求重写equals()方法。-->

- ##### 常用方法：

```java
public void test1(){
    Collection coll = new ArrayList();
    //add：往集合里添加元素
    coll.add("AA");
    coll.add("BB");
    coll.add(123);//自动装箱
    coll.add(new Date());
    //size():获取添加元素的个数
    System.out.println(coll.size());//4
    //addAll(Collection coll1)：将集合coll1中的元素添加到当前的集合中
    Collection coll1 = new ArrayList();
    coll1.add("CC");
    coll1.add(456);
    coll.addAll(coll);
    System.out.println(coll.size());//6
    System.out.println(coll);
    //clear()：清空集合元素
    coll.clear();
    //isEmpty()：判断集合是否为空
    System.out.println(coll.isEmpty());
}

public void test2(){
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add("tom");
        coll.add(false);
        coll.add(new Person("Bob"));
        //contains(Object obj):判断当前集合中是否包含obj。
        boolean contains = coll.contains(123);
        System.out.println(contains);
        System.out.println(coll.contains(new String("tom")));//true，判断的是内容
        //containsAll(Collection coll1)：判断形参coll1中所有元素是都都存在于当前集合中
        Collection coll1 = Arrays.asList(123);
        System.out.println(coll.containsAll(coll1));//true
    }

public void test3(){
        //remove()：从当前集合中移除obj元素
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add("tom");
        coll.add(false);
        coll.add(new Person("Bob"));
        boolean b = coll.remove(123);
        System.out.println(b);//true移除成功
        System.out.println(coll);//123没了
        boolean b1 = coll.remove(new Person("Bob"));
        System.out.println(b1);
        System.out.println(coll);
        //remove()：从当前集合中移除obj元素：从当前集合中移除coll1中所有的元素
        Collection coll1 = new ArrayList(1234);
        coll.removeAll(coll1);
        System.out.println(coll);
    }

    public void test4(){
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add("tom");
        coll.add(false);
        coll.add(new Person("Bob"));
        System.out.println(coll);
        //retainAll(Collection coll1)：交集：获取当前集合和coll1的交集，并返回给当前集合，就是删掉不一样的保留一样的
        Collection coll1 = Arrays.asList(123,3243);
        coll.retainAll(coll1);
        System.out.println(coll);
        //equals(Object obj)：判断当前集合和形参集合的元素是否都相同
        Collection coll2 = new ArrayList();
        coll2.add(123);
        System.out.println(coll.equals(coll2));//true
    }
```

![img](https://img-blog.csdn.net/20180803193423722?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- ##### 迭代器Iterator：


集合元素的遍历操作：使用迭代器Iterator接口

内部的方法：hasNext() 和 next() 方法

```java
public void test(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add("tom");
    coll.add(false);
    coll.add(new Person("Bob"));

    Iterator iterator = coll.iterator();
	//hasNext()：判断是否还有下一个元素
    while(iterator.hasNext()){
        //next()：1、指针下移 2、将下移以后集合位置上的元素返回
        System.out.println(iterator.next());
    }
}
```

![image-20220330225803330](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220330225803330.png)

![image-20220330225906948](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220330225906948.png)

**Iterator**中的**remove()**方法：移除集合中的元素：

如果还未调用next()或在上一次调用next方法之后已经调用remove方法，再调用remove都会报IllegalStateException。

```java
public void test(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add("tom");
    coll.add(false);
    coll.add(new Person("Bob"));

    Iterator iterator = coll.iterator();
    //删除集合中tom数据
    while(iterator.hasNext()){
        Object obj = iterator.next();
        if("tom".equals(obj)){
            iterator.remove();
        }
    }
    //遍历集合
    iterator = coll.iterator();//重新生成迭代器
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
```

- ##### foreach循环：


```java
public void test5() {
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add("tom");
    coll.add(false);
    coll.add(new Person("Bob"));
    //for(集合元素类型 局部变量 ：集合对象)
    //内部仍然调用了迭代器
    for (Object obj:coll) {//每次循环将coll中的值取出来赋值给obj
        System.out.println(obj);
    }
    int[] a = new int[]{1,2,3,4,5};
    for (int i : a) {
    	System.out.println(i);
    }
}
```



#### （一）List接口：

**储存有序的、可重复的数据（“动态”数组，替换原有的数组）**

##### 三种实现类：

（1）**ArrayList**：List的主要接口实现类，线程不安全的，效率高，底层使用**Object[] elementData**存储。

（2）**LinkedList**：对于频繁的插入、删除操作使用此类比ArrayList系哦啊了高，底层使用双向链表存储。

（3）**Vector**：List的古老实现类，线程安全的，效率低，底层使用**Object[] elementData** 存储。

三者相同点：

三个类都是实现了List接口，存储数据的特点相同：储存有序的、可重复的数据。

```java
/*
void add(int index, object ele):在index位置插入ele元素
boolean addAll(int index, collection eles):从index位置开始将eles中的所有元素添加进来 
Object get(int index):获取指定index位置的元素
int index0f(0bject obi):返回obj在集合中首次出现的位置
int lastIndex0f(object obj):返回obj在当前集合中末次出现的位置
Obiect remove(int index):移除指定index位置的元素，并返回此元素 
Object set(int index, object ele):设置指定index位置的元素为ele
List sublist(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合
*/
/*
常用：
增:add(Object obj)
插：add(int index, object ele)
删：remove(int index) / remove(object obj)
改：set(int index, object ele)
查：get(int index)
*/
public void test7(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add("我的list");
        list.add(new Person("tom"));
        System.out.println(list);
        list.add(1,"插入");
        System.out.println(list);
        List list1 = Arrays.asList(1,2,3);
        list.addAll(1,list1);
        System.out.println(list.size());
        System.out.println(list);
        System.out.println(list.get(1));
        System.out.println(list);
        System.out.println(list.remove(5));
        list.remove(new Integer(123));
        list.remove("我的list");
        System.out.println(list);
    }
```



#### （二）Set 接口：

**储存无序的、不可重复的数据（类似数学中“集合”概念）**

<!--向Set中添加的数据，其所在类一定要重写hashCode()方法和equals()方法，重写的hashCode()和 equals()尽可能保持一致性：相等的对象必须具有相等的散列码-->

**特点：**

（1）**无序性：**不等于随机性。存储的数据在数组中并非按照数组的索引顺讯添加，而是根据数据的**哈希值**添加

（2）**不可重复性：**保证添加的元素按照equals()判断时，不能返回true（相同的元素只能添加一个）

##### 三种实现类：

（1）**HashSet：**Set 接口的主要实现类，线程不安全，可以存null值

<!--底层：数组+链表-->

（2）**LinkedHashSet：**HashSet子类，遍历其内部数据时，可以按照添加的顺序遍历，对于频繁遍历的操作，效率高于HashSet

![image-20220401233427145](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220401233427145.png)

（3）**TreeSet：**可以按照添加对象的指定属性，进行排序

<!--向TreeSet中添加数据，要求是相同类的对象-->



**添加元素的过程：以向HashSet中添加元素a为例：**

1、首先调用元素a所在类的hashCode()方法，计算元素a的哈希值

2、此哈希值接着通过某种算法计算出HashSet底层数组中的存放位置(即为索引位置)，判断数组此位置上是否已经有元素

​	（1）没有元素，则a添加成功

​	（2）有元素（或以链表形式存在多个元素）比较a与元素b的hash值：

​			<1>：hash值不相同，则添加成功（以链表形式存储）

​			<2>：hash值相同，进而需要调用元素a所在类的equals()方法：

​					-->equals()返回true，则添加失败（不可重复性）

​					-->equals()返回false，则添加成功（以链表形式存储）



### 二、Map接口

概述：

Map中的key：无序的、不可重复的，使用**Set**存储所有的key。

Map中的value：无序的、可重复的，使用Collection存储所有的value

一个key-value构成了一个Entry对象。

Map中的entry：无序的、不可重复的，使用Set存储所有的key



Map接口和Collection接口的不同：

（1）Map是双列的（一对一对的）,Collection是单列的（一个一个的）

（2）Map的键唯一,Collection的子体系Set是唯一的

（3）Map集合的数据结构针对**键**有效，跟值无关；Collection集合的数据结构是针对**元素**有效。

![image-20220402222046013](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220402222046013.png)

##### Map的实现类

**HashMap**：作为Map的主要实现类，线程不安全，效率高

​		-->LinkedHashMap：保证遍历map元素时，可以按照添加顺序实现遍历，在原有的HashMap底层结构基础上，添加了一对指针，指向前一个元素和后一个元素。

底层：数组+链表+红黑树

![image-20220402225124267](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220402225124267.png)

![image-20220402225217987](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220402225217987.png)

对于频繁的便利操作，执行效率高于HashMap。

**TreeMap：**保证按照添加的**key-value**对进行排序，实现排序遍历，底层使用红黑树。

**Hashtable：**作为古老的实现类，线程安全，效率低，不能存储null的key和value

​		-->**Properties：**常用来处理配置文件，key和value都是String类型

```java
public static void main(String[] args) throws IOException {
    Properties p1 = new Properties();

    FileInputStream fis = new FileInputStream("jdbc.properties");
    p1.load(fis);//加载对应的流文件

    String name = p1.getProperty("name");
    String password = p1.getProperty("password");
    System.out.println(name + "=" + password);
    fis.close();
}
```

##### Map中的常用方法：

![image-20220402215812186](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220402215812186.png)

![image-20220402214252421](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220402214252421.png)

```java
public static void main(String[] args) {
        Map<String,String> map = new HashMap<String,String>();

        map.put("zzw","lxy");
        map.put("zbd","zxb");
        map.put("zyl","xbd");
        map.put("aaa","bbb");
        map.put("ddd","ddd");

        System.out.println(map);
        map.remove("aaa");
        System.out.println(map);
        System.out.println(map.containsKey("zzw"));
        System.out.println(map.containsValue("lxy"));
        System.out.println(map.size());
        System.out.println(map.isEmpty());
    	map.clear();
        System.out.println(map);
      	System.out.println("*******************");
        //根据键获取值
        System.out.println(map.get("zzw"));
        System.out.println(map.get("zbd"));
        System.out.println("*******************");
        //Set<K> keySet()：获取所有键的集合
        Set<String> keySet = map.keySet();
        for(String key:keySet){
            System.out.println(key);
        }
        //Collection<V> values()：获取所有值得集合
        Collection<String> values = map.values();
        for (String value : values) {
            System.out.println(value);
        }
    }
}
```

##### 遍历操作：

```java
public void test1(){
    Map map = new HashMap();

    map.put("AA",123);
    map.put("BB",234);
    map.put("CC",345);
    map.put("DD",456);
    //遍历所有的key集：keySet()
    Set set = map.keySet();
    Iterator iterator = set.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
    //遍历所有的value集：values()
    Collection values = map.values();
    for (Object obj : values) {
        System.out.println(obj);
    }
    //遍历所有的key-value：entrySet()
    //方式一：
    Set entrySet = map.entrySet();
    Iterator iterator1 = entrySet.iterator();
    while(iterator1.hasNext()){
        Object obj = iterator1.next();
        //entrySet集合中元素都是entry
        Map.Entry entry = (Map.Entry)obj;
        System.out.println(entry.getKey() + "-->" + entry.getValue());
    }
    //方式二：
    Set keySet = map.keySet();
    Iterator iterator2 = keySet.iterator();
    while(iterator2.hasNext()){
        Object key = iterator2.next();
        Object value = map.get(key);
        System.out.println(key + "===" + value);
    }
}
```

总结：

```java
/*
添加：put(Object key , Object value)
删除：remove(Object key)
修改：put(Object key, Object value)
查询：get(Object key)
长度：size()
遍历：keySet() 、values()、entrySet()
```























