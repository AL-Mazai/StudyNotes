# IO流

![image-20220405193314970](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220405193314970.png)

![image-20220405193555083](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220405193555083.png)



![image-20220405193828672](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220405193828672.png)



#### IO流的四点明确

##### （1）明确要操作的数据是数据源还是数据目的(要读还是要写)

**源：**
		InputStream　　Reader

**目的：**
		OutputStream　　Writer

##### （2）明确要操作的设备上的数据是字节还是文本

**源：**

字节： InputStream

文本： Reader

**目的：**

字节： OutputStream

文本： Writer

##### （3）明确数据所在的具体设备

**源设备：**

硬盘：文件 File开头

内存：数组，字符串

键盘：System.in

网络：Socket

**对应目的设备：**

硬盘：文件 File开头

内存：数组，字符串

屏幕：System.out

网络：Socket

##### （4）明确是否需要额外功能

需要转换—— 转换流 InputStreamReader 、OutputStreamWriter

需要高效—— 缓冲流Bufferedxxx

多个源—— 序列流 SequenceInputStream

对象序列化—— ObjectInputStream、ObjectOutputStream

保证数据的输出形式—— 打印流PrintStream 、Printwriter

操作基本数据，保证字节原样性——DataOutputStream、DataInputStream

#### 一、字符流

字符流的由来：因为数据编码的不同，因而有了对字符进行高效操作的流对象，字符流本质其实就是基于字节流读取时，去查了指定的码表，而字节流直接读取数据会有乱码的问题（读中文会乱码）。

##### （一）字符流的输入流

```java
//将hello.txt文件内容平读入程序中，并输出到控制台
public void test(){
            //1.实例化File类的对象，指明要操作的文件
            File file = new File("hello.txt");//相较于当前module
            //2.提供具体的流
            FileReader fr = null;
            try {
                fr = new FileReader(file);
                //3.数据的读入
                //read()：返回读入的一个字符，如果达到文件末尾，返回-1
                int data = fr.read();
                while(data != -1){
                    System.out.print((char)data);
                    data = fr.read();
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                //4.流的关闭
                try {
                    if(fr != null)
                    	fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
    /********************方法二***********************/
    //1.实例化File类的对象，指明要操作的文件
        char[] cbuf = new char[10];
        int len;
        while((len = fr.read(cbuf)) != -1){
//            for (int i = 0; i < len; i++) {
//                System.out.print(cbuf[i]);
//            }
            String str = new String(cbuf,0,len);
            System.out.println(str);
        }
        fr.close();
}
```

说明：

1、异常的处理：为了保证流资源一定可以执行关闭操作，需要使用**try-catch-finally**

2、读入的文件一定要存在，否则就会报FileNotFoundException的异常



##### （二）字符流的输出流

```java
public void test() throws IOException {
    //1.实例化File类的对象，指明要操作的文件
    File file = new File("hello.txt");//相较于当前module
    //2.提供具体的流
    FileWriter fw = new FileWriter(file);

    //3.写出操作
    fw.write("I have a dream\n");
    fw.write("you are pig");
    //4.关闭
    fw.close();
}
```

说明：

- 对应的File可以不存在，如果不存在，不会报异常。

- File对应的硬盘中的文件如果不存在：输出过程会自动创建文件；

- File对应的硬盘中的文件如果存在：

  ​	如果流使用的构造器是：FileWriter(file,false)或者FileWriter(file)：对原有文件进行覆盖。

  ​	如果流使用的构造器是：FileWriter(file,true)：不会对原有文件覆盖，而是在原有文件基础上追加内容。

  

  **输入输出的综合：**

```java
public void test2(){
    //1.实例化File类的对象，指明要读入和写出的对象
    File srcFile = new File("hello1.txt");
    File destFile = new File("hello2.txt");
    //2.创建输入流和输出流的对象
    FileReader fr = null;
    FileWriter fw = null;
    try {
        fr = new FileReader(srcFile);
        fw = new FileWriter(destFile);
        //3.读入和写出操作
        char[] cbuf = new char[5];
        int len;
        while((len = fr.read(cbuf)) != -1){//从硬盘中写入内存
            //每次写出len个字符
            fw.write(cbuf,0,len);//从内存中写出到硬盘中
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            //4.关闭
            if(fw != null)
                fw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if(fr != null)
                fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



#### 二、字节流

`java.io.OutputStream`抽象类是表示**字节输出流**的所有类的**超类**（父类），将指定的字节信息写出到目的地。它定义了字节输出流的基本共性功能方法。

##### 字节流输入输出的例子（没有处理异常）：

```java
public void test4(){

    File srcfile = new File("zzw.jpg");
    File destfile = new File("zzw1.jpg");

    FileInputStream fi = new FileInputStream(srcfile);
    FileOutputStream fo = new FileOutputStream(destfile);

    byte[] buffer = new byte[50];
    int len;
    while((len = fi.read(buffer)) != -1){
        fo.write(buffer,0,len);
    }

    fi.close();
    fo.close();

}
```

**总结：**

1、对于文本文件（.txt、.java、.c、.cpp)使用字符流处理

2、对于非文本文件(.jpg、.mp3、.doc)使用字节流处理

#### 三、缓冲流

**作用：提高流的读取、写入的速度**

##### 原理：

1、使用了底层流对象从具体设备上获取数据，并将数据存储到缓冲区的数组内。

2、通过缓冲区的read()方法从缓冲区获取具体的字符数据，这样就提高了效率。

3、如果用read方法读取字符数据，并存储到另一个容器中，直到读取到了换行符时，将另一个容器临时存储的数据转成字符串返回，就形成了readLine()功能。

##### （一）字节型缓冲流：

```java
    public void test5() throws FileNotFoundException {
        FileInputStream fi = null;
        FileOutputStream fo = null;
        BufferedInputStream bi = null;
        BufferedOutputStream bo = null;
        try {
            //1.造文件
            File srcfile = new File("zzw.jpg");
            File destfile = new File("zzw1.jpg");
            //2.造流
            //2.1、造节点流
            fi = new FileInputStream(srcfile);
            fo = new FileOutputStream(destfile);
            //2.2、造缓冲流
            bi = new BufferedInputStream(fi);
            bo = new BufferedOutputStream(fo);
            //3.复制的细节：读取、写入
            byte[] buffer = new byte[10];
            int len;
            while((len = bi.read(buffer)) != -1){
                bo.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭：先关闭外层的流，再关闭内层的流（好比穿衣服）
            try {
                bo.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            
            try {
                bi.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            //说明：关闭外层流的同时，内层流也会自动关闭
//            fi.close();
//            fo.close();
        }
    }
```



##### （二）字符型缓冲流：

```java
public void test6() throws FileNotFoundException {
        FileReader fr = null;
        FileWriter fw = null;
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
            //1.造文件
            File srcfile = new File("hello.txt");
            File destfile = new File("hello1.txt");
            //2.造流
            //2.1、造节点流
            fr = new FileReader(srcfile);
            fw = new FileWriter(destfile);
            //2.2、造缓冲流
            br = new BufferedReader(fr);
            bw = new BufferedWriter(fw);
            //3.复制的细节：读取、写入
            char[] cbuf = new char[10];
            int len;
            while((len = br.read(cbuf)) != -1){
                bw.write(cbuf,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭：先关闭外层的流，再关闭内层的流（好比穿衣服）
            try {
                br.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                bw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            //说明：关闭外层流的同时，内层流也会自动关闭
//            fi.close();
//            fo.close();
        }
    }
```



#### 四、转换流

计算机中储存的信息都是用二进制数表示的，而我们在屏幕上看到的数字、英文、标点符号、汉字等字符是二进制数转换之后的结果。

- 按照某种规则，将字符存储到计算机中，称为**编码** 

- 将存储在计算机中的二进制数按照某种规则解析显示出来，称为**解码** 

<!--编码：字符(能看懂的)–字节(看不懂的)-->

<!--解码：字节(看不懂的)字符(能看懂的)-->

比如说，按照`A`规则存储，同样按照`A`规则解析，那么就能显示正确的文本符号。反之，按照`A`规则存储，再按照`B`规则解析，就会导致乱码现象。

![img](https://img-blog.csdnimg.cn/2019101609401732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NTQzNTA4,size_16,color_FFFFFF,t_70)

<!--属于字符流-->

```
InputStreamReader：将一个字节的输入流转换为字符的输入流
OutputStreamWriter：将一个字符的输出流转换为字节的输出流
```

![image-20220408171444404](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220408171444404.png)



```java
public void test8() throws IOException {
    File file1 = new File("hello.txt");
    File file2 = new File("hello1.txt");

    FileInputStream fis = new FileInputStream(file1);
    FileOutputStream fos = new FileOutputStream(file2);

    InputStreamReader isr = new InputStreamReader(fis,"UTF-8");//解码
    OutputStreamWriter osw = new OutputStreamWriter(fos,"GBK");//编码

    char[] cbuf = new char[10];
    int len;
    while((len = isr.read(cbuf)) != -1){
        osw.write(cbuf,0,len);
    }

    isr.close();
    osw.close();

}
```

