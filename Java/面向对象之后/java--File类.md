# File类

##### 1、创建File类：

```java
public void test1(){
    //构造器1:File(String pathname)
    File file1 = new File("hello.txt");//相对于当前module
    File file2 = new File("E:\\IDEA_java_practice\\Project1\\Shangguigu\\src\\zzw\\java\\file");
    System.out.println(file1);
    //构造器2:File(String parent, String child)
    File file3 = new File("E:\\IDEA_java_practice","java");
    System.out.println(file3);
    //构造器3:File(File parent, String child)
    File file4 = new File(file3,"javaSE");
    System.out.println(file4);
}
```

**相对路径：相对于某个路径下，指明的路径**

**绝对路径：包含盘符在内的文件或者文件目录的路径**

##### 2、常用方法：

```java
public string getAbsolutePath():获取绝对路径
public string getPath():获取路径 
public string getName():获取名称
public string getParent():获取上层文件目录路径。若无,返回null
public long length():获取文件长度(即:字节数)。不能获取目录的长度。 
public long lastModified():获取最后一次的修改时间,毫秒值
//适用于文件目录
public string[] list():获取指定目录下的所有文件或者文件目录的名称数组
public File[] listFiles():获取指定目录下的所有文件或者文件目录的File数组
boolean renameTo(File dest):把文件重命名为指定的文件路径
/******************************************************/
public boolean isDirectory():判断是否是文件目录 
public boolean isFile():判断是否是文件 
public boolean exists():判断是否存在 
public boolean canRead():判断是否可读 
public boolean canWrite():判断是否可写 
public boolean isHidden():判断是否隐藏
/******************************************************/
创建硬盘中对应的文件或者文件目录：
public boolean createNewFile():创建文件。若文件存在，则不创建，返回false
public boolean mkdir():创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建
public boolean mkdirs():创建文件目录。如果上层文件目录不存在，一并创建
public boolean delete():删除文件或者文件夹
//删除注意事项:Java中的删除不走回收站。
```

<!--File类中涉及到文件或者文件目录的创建、删除、重命名、修改时间、文件大小等方法，并未涉及到写入或者读取文件内容的操作（要使用IO流）。File类的对象常会作为参数传递到流的构造器中，指明读取或者写入的“终点”-->