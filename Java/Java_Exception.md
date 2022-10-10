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

​		printStackTrace()体现了给错误起名的重要性（下文会提到）

​		我们可以对异常进行转化

```java
void process1(String s) {
  try {
    process2();
  }catch(NullPointerException e) {
    throw new IllegalArgumentException();
  }
}

void process2(String s) {
  if (s == null) {
    throw new NullPointerException();
  }
}
```

​		可以看出 process1对process2产生的`NullPointerException`进行捕获， 并转化								为`IllegalArgumentException` 继续抛出

​		查看完整程序:

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException();
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}

```

​		查看该程序异常栈:

```java
java.lang.IllegalArgumentException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
```

​		发现我们没有找到process2中抛出的 `NullPointerException()` 。异常栈只打印出了在Main中就		地发现的Process1抛出的 IllegalArgumentException 这显然是不完整的。解决方法如下

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e); //这里新增加了一个e 是将e转化了
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}

```

​		这样打印出的异常栈如下

```java
java.lang.IllegalArgumentException: java.lang.NullPointerException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
Caused by: java.lang.NullPointerException
    at Main.process2(Main.java:20)
    at Main.process1(Main.java:13)
```

​		这里明显多出了caused by这行。说明，上面的IllegalArgumentException并不是真正原因，这个错		误是由 NullPointerException所cause的。

​		如果想直接溯源，可以将括号中的Exception e改为 Throwable e，并将e.getCause()打印出来

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Throwable e) { // 差别在这里
            System.out.println(e.getCause()); // 差别在这里
            e.printStackTrace();

        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e);
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}

```

​		另外，异常的抛出不影响finally的执行。因为jvm会先执行finally，再抛出异常

4. 异常屏蔽

   如果在执行finally语句的时候抛出异常，那么，catch语句的异常不能被抛出，如下

   ```java
   public class Main {
       public static void main(String[] args) {
           try {
               Integer.parseInt("abc");
           } catch (Exception e) {
               System.out.println("catched");
               throw new RuntimeException(e);
           } finally {
               System.out.println("finally");
               throw new IllegalArgumentException();
           }
       }
   }
   //catched
   //finally
   //Exception in thread "main" java.lang.IllegalArgumentException
   //		at Main.main(Main.java:11)
   ```

   说明 在catch到try中的异常后，sout可以先执行，输出catched，然后不会急着抛出异常，会先运行finally。finally的sout输出finally后，会优先抛出finally中的IllegalArgumentException，然而，一段程序的一个分支只能抛出一个异常，所以catch中的异常并没有被抛出，也就是catch中的异常被finally中的异常***屏蔽***了。

   然而，在少数情况下，我们想要知道所有的异常。那么如何保存所有的异常信息？

   方法是用`origin`保存原始异常，然后调用`Throwable.addSuppressed()`把原始异常添加进来，最后用finally抛出。

   原理是，异常屏蔽是屏蔽throw这一过程，但不会屏蔽throw之前的过程。我们及时将catch想要抛出的错误存入一个作用域中的错误对象，在finally的作用范围内，奖catch中的错误add入finally的错误中，这样由finally一并抛出。这样就可以将两个异常都抛出了

   ```java
   public class Main {
       public static void main(String[] args) throws Exception {
           Exception origin = null;
           try {
               System.out.println(Integer.parseInt("abc"));
           } catch (Exception e) {
               origin = e;
               throw e;
           } finally {
               Exception e = new IllegalArgumentException();
               if (origin != null) {
                   e.addSuppressed(origin);
               }
               throw e;
           }
       }
   }
   -------------------------------------------------------
   Exception in thread "main" java.lang.IllegalArgumentException
       at Main.main(Main.java:11)
   Suppressed: java.lang.NumberFormatException: For input string: "abc"
       at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
       at java.base/java.lang.Integer.parseInt(Integer.java:652)
       at java.base/java.lang.Integer.parseInt(Integer.java:770)
       at Main.main(Main.java:6)
   
   ```

   通过`Throwable.getSuppressed()`可以获取所有的`Suppressed Exception`。

5. 自定义异常

```
Exception
│
├─ RuntimeException
│  │
│  ├─ NullPointerException
│  │
│  ├─ IndexOutOfBoundsException
│  │
│  ├─ SecurityException
│  │
│  └─ IllegalArgumentException
│     │
│     └─ NumberFormatException
│
├─ IOException
│  │
│  ├─ UnsupportedCharsetException
│  │
│  ├─ FileNotFoundException
│  │
│  └─ SocketException
│
├─ ParseException
│
├─ GeneralSecurityException
│
├─ SQLException
│
└─ TimeoutException
```



​		以上是我们常用的异常 这显然是不够用的。

​		在项目中，我们可以自定义新的异常类型。但是要符合合理的继承体系。

​		一个常见的做法是自定义一个BaseException作为根异常，然后派生出各种类型的异常。

​		BaseException需要从一个适合的Exception派生。我们建议从RuntimException派生

​		推荐从RuntimeException派生的原因是，不用强制 try catch。如果是其他类型的错误，一旦出现		就会终止程序，这样子流程就会断掉。

```java
public class BaseException extends RuntimeException{}
```

​		而其他类型的异常就可以由BaseException派生

```java
public class UserNotFoundException extends BaseException(){}
```

​		自定义的BaseException应该提供多个构造方法。

```java
public class BaseException extends RuntimeException {
    public BaseException() {
        super();
    }

    public BaseException(String message, Throwable cause) {
        super(message, cause);
    }

    public BaseException(String message) {
        super(message);
    }

    public BaseException(Throwable cause) {
        super(cause);
    }
```

6. NullPointerException

   NPE空指针异常，java中没有指针，只有引用。Null Pointer应该纠正为Null Reference, 不过区别不大，懂的都懂。

   我们***不应该***去 try catch NPE， 因为NPE是一种代码上的逻辑错误，所以这种错误应该早暴露，早修复，所以就不catch了。

   避免NPE的编码习惯：

   1. 成员变量在定义时就做好初始化

   2. 使用空字符串“” 而不是null

   3. 如果调用方一定要根据null判断，建议使用Optional类来解决空指针异常。

      ```java
      public Optional<String> readFromFile(String file) {
          if (!fileExist(file)) {
              return Optional.empty();
          }
          ...
      }
      ```

   如果产生了NullPointerException，JVM可以告诉我们null的到底是什么

   Idea,Run->EditConfigurations-> VM options

   ```java
   java -XX:+ShowCodeDetailsInExceptionMessages Main.java
   ```

7. 断言

   ```java
   public static void main(String[] args) {
       double x = Math.abs(-123.45);
       assert x >= 0 : "x must >= 0"; //这就是断言
       System.out.println(x);
   }
   ```

​		如果断言失败，则会抛出`AssertionError`

8. 使用JDK Logging (TODO)

   我们都会在代码中加入一些sout代码去打印一些中间变量，进行调试。但是，调试好之后，我们又要删除sout，又出现问题，那就又要加上sout。这十分麻烦。

9. 使用Commons Logging(TODO)

10. 使用Log4j(TODO)

11. 使用SLF4J今儿Logback(TODO)
