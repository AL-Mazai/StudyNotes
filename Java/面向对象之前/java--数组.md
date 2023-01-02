# 数组

##### 1.声明和初始化

静态初始化：数组的初始化和数组元素赋值操作同时进行

```java 
int[] array;//一维数组声明
array = new int[]{1001,1002,1003,1004};

int[][] array;//二维数组声明
//或者 int[] array[] 
array = new int[][]{{1,2,3},{2,3,4}，{3,4,5}};
```

动态初始化：数组的初始化和数组元素的赋值操作分开进行

```java
String[] names = new String[5];
```



##### 2.获取数组长度（属性：长度）

```java
String[] names = new String[5];
System.out.println(names.length);//5
```



##### 3.数组元素默认初始值

<!--按数组元素类型分类-->

(1)一维数组

整型：0

浮点型：0.0

char型：0或 '\u0000'，不是'0'

boolean型：false

引用数据类型：null

(2)二维数组

初始化方式一：

```java
int[][] arr = new int[4][3];
```

外层元素：地址值

内层元素：与一维数组一样



初始化方式二：

```java
int[][] arr = new int[4][];
```

外层元素：null

内层元素：不能调用