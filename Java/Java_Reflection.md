# Reflection

反射是框架的灵魂。没有反射就没有后面的一系列框架。

***反射就是在运行中才知道要运行的类是什么，并且在运行时获取类的完整构造，并调用其方法***

---



### **什么是反射？**

我们追求，不要把程序写死，这是动态语言的灵魂之一。如果我们把数据库地址，用户名，密码之类的东西全部写死在程序中。

一方面，容易「泄露」，前段时间上海的公民信息库就是把数据库有关信息写在程序中，被莫名其妙开源而导致泄露。

另一方面，如果你不是个人编程，而是在公司，要把产品卖给客户，那么客户的用户名，密码，数据库的信息不可能与你一样，我们不可能为每一个客户都去修改一份程序，这太累了。所以我们经常把这些「配置信息」写在类似于`properties` 或者`json`之类的配置文件中，到时候动态获取。但是，如果我们通过Filereader来读取文件中的东西，我们得到的返回值是一个Object，当然你可以使用toString()方法去将其转化为字符串，以供使用，但是，如果你获得一个String，后面再加上()之类的东西，编译器不会将其理解为是一种方法，而是你纯纯的语法错误，没有人这么写，除了使用***PHP这种世界上最美的语言的***人，这种方法在PHP中确实是被允许的。所以我们需要一种动态去获取对象的方法。

接下来会写一个例子，这个例子可以让我立刻想起来反射到底是什么东西，希望你也可以理解

第一段是一个**配置文件**，pr.properties

```properties
//pr.properties
class=com.indexss.cat.cat
method=hi
```

第二段是**反射实例** com.indexss.cat.cat

```java
package com.indexss.cat;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Properties;

public class cat {
    String name = "helo";
    public void hi(){
        System.out.println("hi, there");
    }
}

class test{
    public static void main(String[] args) throws IOException, ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
        Properties properties = new Properties();
        properties.load(new FileReader("src/pr.properties"));
        String clsName = properties.get("class").toString();
        String methodName = properties.get("method").toString();

        System.out.println(clsName);
        System.out.println(methodName);
        Class cls = Class.forName(clsName);
        Object o = cls.newInstance();
        System.out.println("o的运行环境" + o.getClass());
//        Method m1 = cls.getMethod("methodName"); 这样写会报错，不应该添“”
//        m1.invoke(o);
        Method method1 = cls.getMethod(methodName);
        method1.invoke(o);

    }
}

```

---

反射机制允许程序在执行中借助于Reflection API去的任何累的内部信息，并能操作对象的属性以及方法。

## 反射名字的由来

加载完类之后，在堆中就产生了一个Class类型的对象（一个类只有一个class对象），这个对象包含了类的完整结构信息，通过这个对象得到了类的结构。但是这个对象并不是你要操作的那个类，而是类的一个**「影像」**，一种类似于**「孪生」**的关系，就像一面镜子，可以透过镜子看到看到类的结构，所以称之为反射。

## 反射可以干什么

1. 在运行时判断任意一个对象所属的类
2. 在运行时构造任意一个类的对象
3. 在运行时得到任意一个类所具有的成员变量和方法
4. 在运行时调用任意一个对象的成员变量和方法
5. 生成动态代理



1. Class类

   除了基本类型（int, char）， Java其他类型全是class类，包括interface

   `String` `Object` `Runnable` `Exception`

   没有继承关系的数据类型无法赋值。

   ```java
   Number n =  new Double(123.456); // ok
   String s = new Double(123.456); // compile error
   ```

   
