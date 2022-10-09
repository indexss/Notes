# Java异常处理

**在异常处理中 我们一般采用两种方式：**

- 方法一： 返回错误码

  ```java
  int code = processFile("url");
  if (code == 0){
    //ok
  }else{
    switch(code){
      case 1:
        // file not found
      case 2:
        //no read permisson
      default:
        //unknown error
    }
  }
  ```

  这种int类型的错误码 想要处理是一件很麻烦的事情 经常见于C语言

- 方法二：提供语言层面的异常处理机制

  ```java
  try{
    String s = processFile("url");
  }catch(FileNotFoundException e) {
    // file not found:
  }catch(SecurityException e) {
    // no read permission:
  }catch(IOException e) {
    // io error:
  }catch (Exception e) {
    // other error:
  }
  ```

  <img src="https://s3.bmp.ovh/imgs/2022/10/08/fb196d492dc385c2.png" alt="javaClassError" style="zoom: 50%;" />

这张图是Java Exception的分类 可以记录一下

1. 通过 `try{}catch(Exception ex){}` 来预判 捕获异常
2. 可以通过多catch语句捕获对应的Exception 多catch语句只有一个能被执行

​		如果catch之后执行的命令一样 可以使用`|`这个符号来将多种异常隔开

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException | NumberFormatException e) { 
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```



​		如果不管是否有异常发生 我们都希望执行一些语句 怎么办？

​		使用 `finally` 语句 finally一般包括的都是一些清扫工作

​		有以下两个特点

​		        - finally语句不是必须的 可写可不写

​				- finally总是最后执行

​		如果没有发生异常 那就正常执行try 再执行finally。 如果发生了异常 那就中断执行try，跳到要执行		的catch，最后执行finally

3. 异常的抛出

   ```java
   void process1(String s) {
       try {
           process2();
       } catch (NullPointerException e) {
           throw new IllegalArgumentException();
       }
   }
   
   void process2(String s) {
       if (s==null) {
           throw new NullPointerException();
       }
   }
   ```

​		可以通过 `printStackTrace()` 来打印出方法的调用栈

```java
try{}catch(Exception ex){
  ex.printStackTrace();
}
```

​		这样就会打印出调用信息。 这种信息可以从下向上看，即为调用顺序

