#### String类型变量的使用

1.String属于引用数据类型,翻译为:字符串

2.声明String类型变量时，使用一对 ""

3.String可以和8种基本数据类型变量做运算，且运算只能是连接运算:+（跟String类型进行的+都是表示连接)

4.运算的结果仍然是String类型

```java 
class  test
{
	public static void main(String[] args) 
	{
		String s = "我的年龄：";
		int age =100;
		String info = s + age;//+:连接符号(跟String类型进行的+都是表示连接)
		System.out.println(info);
		//info:我的年龄：100

	}
}

```

