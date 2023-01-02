# equals的使用

Object类中的equals方法用于检测一个对象是否等于另一个对象。在Object类中，这个方法判断两个对象是否具有相同的引用，如果两个对象具有相同的引用，它们一定是相等的。从这点上看，将其作为默认操作也是合乎情理的。然而，对于多数类类说，这种判断并没有什么意义，例如，采用这种方式比较两个PrintStream是否相等就完全没有意义。然而，经常需要检测两个对象状态的相等性，如果两个对象的状态相等，就认为这两个对象是相等的。所以一般在自定义类中都要重写equals比较。

##### 重写equals()方法：

1、显式参数命名为 otherObject，稍后需要将转换成一个叫other的变量

2、检测 this 与 otherObject 是否引用同一个对象：

```java
if(this==otherObject) return true
```

这条语句只是一个优化。实际上，这是一种经常采用的形式。因为计算这个等式要比一个一个地比较类中的域所付出的代价小的多。

3、检测otherObject是否为null，如果为null，返回false。这项检测是很必要的。

```java
if(otherObject==null) return false;
```

4、比较 this 和 otherObject 是否属于同一个类，如果equals的语义在每个子类中有所改变，就使用 getClass() 检测，它将自己作为目标类：

```java
if(getClass()!=otherObject.getClass()) return false;
```

如果所有的子类都拥有同一的语义，就使用 **instanceof** 检测

```java
if(!(otherObject instanceof ClassName)) return false;
```

5、将 otherObject 转换为相应类型的变量：

```java
ClassName other = (ClassName)otherObject;
```

6、现在开始对所有需要比较的域进行比较。使用 **==** 比较基本类型域（基本数据类型），使用**equals** 比较对象域（引用数据类型）。如果所有域都匹配，就返回true，否则返回 false：

```java
return field1 == other.field1 && field2.equals(other.field2)
```

如果在子类中重新定义 equals，就要在其中包含调用 super.equals(other)。如果检测失败，就不可能相等。如果超类中的域相等，就比较子类中的实例域。

![image-20220224115441728](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220224115441728.png)

