##### 基本数据类型之间的运算规则:

<!--前提:这里讨论只是7种基本数据类型变量间的运算。不包含boolean类型的。-->

##### 1.自动类型提升:

结论:当容量小的数据类型的变量与容量大的数据类型的变量做运算时，结果自动提升为容量大的数据类型

byte 、char 、short --> int --> long --> float --> double

<!--特别的:当byte、char、short三种类型的变量做运算时，结果为int型-->

##### 2.强制类型转换：自动类型提升运算的逆运算

1、需要使用强转符：（）

2、注意点：强制类型转换，可能导致精度损失。

```Java
class  test
{
	public static void main(String[] args) 
	{
        //精度损失
		double d1-12.9;
        int i1=(int)d1;//截断操作
        System.out.println(i1);//i1=12
        
        //精度没有损失
        long l=111;
        short s=(short)l;//s=111
        
        
	}
}

```

<!--整形常量，默认类型为int类型-->

<!--浮点型常量，默认类型为double类型-->

<!--说明:此时的容量大小指的是，表示数的范围的大和小。比如:float容量要大于long的容量-->