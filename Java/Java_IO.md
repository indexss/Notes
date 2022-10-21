# Java IO

> IO是指Input/Output,也就是输入和输出，以内存为中心。

我们站在内存的角度分输入和输出。

将外部数据读入到内存的设备就是输入设备，相反地，从内存读出的设备就是输出设备

IO Stream以byte为最小单位。

Java提供了Reader和Weiter表示字符流，字符流传输的最小数据单位是char。

## 1、File对象

Java标准库java.io提供了File对象来操作文件和目录。

要构造一个File对象，需要传入文件路径:

```java
package com.indexss.IO;
import java.io.File;
import java.io.IOException;
public class ioTest {
    public static void main(String[] args) throws IOException {
        File file = new File("/Users/shilinli/Desktop/Projects/javaProjects/GUI2/src/com/indexss/snakegame/pics/snakeUp.png");
        System.out.println(file.getPath());     //  返回构造器传入的路径
        System.out.println(file.getAbsolutePath());  //  返回绝对路径
        System.out.println(file.getCanonicalPath());  //  最简路径，简化拓扑
        System.out.println(file.separator);  //  打印当前系统分隔符
    }
}

```

File对象既可以表示文件，也可以表示目录，但是，就算你提供过的目录非法，也不会报错，因为当我们构造File对象的时候，并没有对这个路径做什么操作。只有当我们调用File对象的某些方法的时候，才能真正进行磁盘操作。

我们可以调用`isFile()`来判断File对象是不是一个已经存在的文件，调用`isDirectory()`,可以判断这个目录是否合法

```java
System.out.println(file.isFile());
```

## 	创建和删除文件

当File对象表示一个文件的时候，我们可以通过createNewFile() 方法创造一个新文件，用delete删除文件

```java
public static class Main(String[] args){
  File file = new File("./pathTest");
  if(file.createNewFile()){
    //suscessfully create
    if(file.delete()){
      //susccesfully delete
    }
  }
}
```

我们有些时候想要创建一些临时文件，File对象提供了createTempFile()来创造临时文件，以及当JVM退出的时候就删除文件的命令deleteOnExit()

```java
package com.indexss.IO;
import java.io.File;
import java.io.IOException;
public class testTempFile {
    public static void main(String[] args) throws IOException {
        File tempFile = File.createTempFile("tmp-", ".txt");
        tempFile.deleteOnExit();
        System.out.println(tempFile.isFile());
        System.out.println(tempFile.getAbsolutePath());
    }
}
```

##   遍历文件和目录



