1. 字符串比较用 s1.equals(s2) 忽略大小写用equalsIgnoreCase()
2. indexOf() lastindexOf() startsWith("he") endsWith("lo")
3. s1.substring(2); s1.substring(start,end)
4. 去除收尾空白 trim() 去除中文空白strip() 
5. isEmpty()空？ isBlank() 空白（无信息）？
6. 替换字串 s1.replace("aa", "bb");     s1.replaceAll("正则", "被替换的东西")
7. 分割字符串 string[ ] ss = s.split("正则");
8. 拼接字符串 String[ ] arr = {"A", "B", "C"}；

​						   String s = String.join("***, arr");

9. formatted()    or.      format()

```java
public class Main(){
  public static void main(String[] args){
    String s = "Hi %s, your score is %d";
    System.out.println(s.formatted("Alice, 80"));
    System.out.println(String.format("Hi %s, your name is %.2f"), "Bob". 92);
  }
}
```

10. 类似cpp的自动转换格式 别人转字符串 String.valueOf(123) 从123转化为String

    ​											String.valueOf(true) 从true变为"true"

11. 字符串转别人 parse就完事

    int n1 = Integer.parseInt("123");

    int n2 = Integer.parseInt("ff", 16)  ////按照16进制转化

    

12. getInteger:     Integer.getInteger("java.version"); // 版本号，11

13. ```java
    char[] cs = "Hello".toCharArray(); // String -> char[]
    String s = new String(cs); // char[] -> String
    //对char[]的操作不会影响s 因为是复制版本
    ```

14. 高效拼接字符串 StringBuilder 往StringBuilder中新增字符时，不会创建新的临时对象 大小固定的字符串

15. 高效添加分割符 StringJoiner

    ```java
    public class Main {
        public static void main(String[] args) {
            String[] names = {"Bob", "Alice", "Grace"};
            var sj = new StringJoiner(", ");
            for (String name : names) {
                sj.add(name);
            }
            System.out.println(sj.toString());
        }
    }
    //也可以给开头和结尾
    public class Main {
        public static void main(String[] args) {
            String[] names = {"Bob", "Alice", "Grace"};
            var sj = new StringJoiner(", ", "Hello ", "!");
            for (String name : names) {
                sj.add(name);
            }
            System.out.println(sj.toString());
        }
    }
    ```

​		也可以用另一种方法来添加分割符 String的静态方法join()

```java
String[] names = {"Bob", "Alice", "Grace"};
var s = String.join(", ", names);
```

16. 包装类型  引用类型 如String是可以复制null的 表示空 但是int这种基本类型就不行 所以java为这种基本类型也做了包装 java提供了自动装箱，自动拆箱。

    这种直接把`int`变为`Integer`的赋值写法，称为自动装箱（Auto Boxing），反过来，把`Integer`变为`int`的赋值写法，称为自动拆箱（Auto Unboxing）。

    ```java
    Integer n = 100;
    int x = n; //实际上 n和x的类型不是一样的 但是可以互相转型 所以说叫自动装箱和自动拆箱
    //创建Integer类的时候 使用valueOf而不是直接new 因为这种事静态工厂方法 可以节约内存	
    Integer n = Integer.valueOf(100);
    ```

    引用类型需要用equals()比较 

    Integer对象是final类的 也就是不变类 所以一旦创建了Integer对象 那么这个对象就是不变的

    Integer还提供了一些好用的方法 最常用高的静态方法parseInt()可以把字符串解析为整数

    ```java
    int x1 = Integer.parseInt("100");
    int x2 = Integer.parseInt("100", 16); //按照16进制解析
    Integer n3 = Integer.valueOf("100");
    System.out.println(n3.intValue());
    ```

    由于所有的整数和浮点数的包装类型都继承自Number 所以，可以用向上转型的方法

    ```java
    Number num = new Integer(999);
    byte b = num.byteValue();
    int n = num.intValue();
    long ln = num.longValue();
    float f = num.floatValue();
    double d = num.doubleValue();
    
    ```

    还可以处理无符号类型 

17. JavaBean

    如果class的定义符合以下规范：

    		- 通过private来实例化字段
		
    		- 通过public方法来获得字段信息

    如

    ```java
    public class Person {
        private String name;
        private int age;
    
        public String getName() { return this.name; }
        public void setName(String name) { this.name = name; }
    
        public int getAge() { return this.age; }
        public void setAge(int age) { this.age = age; }
    }
    ```

    那么这种class就被称作JavaBean

    boolean字段较为特殊 它的命名是用isXyz()来命名的

    ```java
    public boolean isChild(){}
    public void setChild(boolean value){}
    ```

    get叫做getter 读方法 set叫做setter 写方法 他们读写的东西叫做属性

    只有getter就是只读 只有setter就是只写（不常见）

18. 枚举类

    常量写起来很麻烦 public static final int SUN = 0;

    如果写if(常量 == number)的话 number有可能不在常量的规定范围内，也有可能是magic number

    故做一个枚举类

    ```java
    public class Main {
        public static void main(String[] args) {
            Weekday day = Weekday.SUN;
            if (day == Weekday.SAT || day == Weekday.SUN) {
                System.out.println("Work at home!");
            } else {
                System.out.println("Work at office!");
            }
        }
    }
    
    enum Weekday {
        SUN, MON, TUE, WED, THU, FRI, SAT;
    }
    ```

    使用enum有自检查的功能 

    由于Weekday.SUM 的类型是Weekday 故你不能这样写代码

    ```java
    int day = 1;
    if(day == Weekday.SUM){//Compile erroe: bad operand types for binary operator "=="
    }
    ```

    枚举类不能串赋值

    ```java
    Weekday x = Weekday.SUN; //OK
    Weekday y = Color.RED //Compile error
    ```

    由于enum的常量在jvm中只有一个实例 	所以可以用==比较

    enum是 class的子类 有以下几个特点

    - 定义的enum类型继承自java.lang.Enum 无法被继承
    - 只能定义出enum的实例，且无法new来实例化
    - 每个实例都是引用类型的唯一实例 所以可以用==比较
    - 可以用于switch语句

    一种使用private的构造方法 给每个枚举常量添加字段

    由于枚举是在类内实例化的 所以可以这样写

    ```java
    public class Main{
        public static void main(String[] args) {
            int task = Weekday.MON.dayValue;
            System.out.println(task);
        }
    }
    enum Weekday{
        SUN(6), MON(0), TUE(1), WED(2), THR(3), FRI(4), SAT(5);
        public final int dayValue;
        private Weekday(int datValue){
            this.dayValue = datValue;
        }
    }
    ```

    对枚举常量使用toString()会返回和name()一样的字符串 但是toString()可以被重写，但是name不行

    19. 记录类

        用String Integer等类型的时候 这些类型都是不变类 不变类有以下两个特点

        - 定义class时使用final 无法派生子类
        - 每个字段使用final 保证创建实例后无法修改任何字段

        如果希望有一个point类 有x y两个变量 同时是一个不变类 可以这么写:

        ```java
        public final class Point{
          private final int x;
          private final int y;
          
          public Point(int x, int y){
            this.x = x;
            this.y = y;
          }
          
          public int x(){
            return this.x;
          }
          
          public int y(){
            return this.y;
          }
        }
        ```

        为了保证不变类的比较 还需要正确重写equals 和hashCode方法 这样才能在集合类中正常使用。

        而record类就不存在这方面的问题 而且写起来比较简单

        ```java
        public record Point(int x, int y){}
        ```

        这段代码虽然很短 但是如果写成class的话 效果是这样的

        ```java
        public final class Point extends Record {
            private final int x;
            private final int y;
        
            public Point(int x, int y) {
                this.x = x;
                this.y = y;
            }
        
            public int x() {
                return this.x;
            }
        
            public int y() {
                return this.y;
            }
        
            public String toString() {
                return String.format("Point[x=%s, y=%s]", x, y);
            }
        
            public boolean equals(Object o) {
                ...
            }
            public int hashCode() {
                ...
            }
        }
        ```

        使用不变类的好处就是 ： **可以一行写出一个不变类**

        record类的compact constructor **帮助我们检查逻辑**

        ```java
        public record Point(int x, int y){
          public Point{
            if(x < 0 || y < 0){
              throw new ILLegalArgumentException();
            }
          }
        }
        ```

        这行compact constructor 没有圆括号 区分与普通构造器的区别

        若将compact constructor 改写为class形式 如下

        ```java
        public final class Point extends Record {
            public Point(int x, int y) {
                // 这是我们编写的Compact Constructor:
                if (x < 0 || y < 0) {
                    throw new IllegalArgumentException();
                }
                // 这是编译器继续生成的赋值代码:
                this.x = x;
                this.y = y;
            }
            ...
        }
        ```

        record还有更简洁的写法

        ```java
        public class Main{
            public static void main(String[] args) {
                var z = Point.of();
                var p = Point.of(3,4);
            }
        }
        record Point(int x, int y){
            public static Point of(){
                return new Point(0, 0);
            }
            public static Point of(int x, int y){
                return new Point(x, y);
            }
        }
        ```

20. BigInteger

    java中 硬件最大整形是64位的long型整数 。

    若超过long型 就要用到软件模拟

    java中就是用java.math.BigInteger来表示任意大小的整数。

    其内部原理是用一个很大的int[]来模拟一个非常大的整数

    ```java
    import java.math.BigInteger;
    public class Main{
        public static void main(String[] args) {
            BigInteger bi = new BigInteger("1234567890");
            System.out.println(bi.pow(10));
        }
    }
    ```

​		对BigInteger操作 只能使用实例方法 比如加法运算

​	

```java
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("1234567899719047");
BigInteger sum = i1.add(i2);
```

- 转换为`byte`：`byteValue()`
- 转换为`short`：`shortValue()`
- 转换为`int`：`intValue()`
- 转换为`long`：`longValue()`
- 转换为`float`：`floatValue()`
- 转换为`double`：`doubleValue()`

20. BigDecimal

    `BigDecimal`是可以表示任何精度 且完全准确的浮点数

    ```java
    BigDecimal bd = new BigDecimal("123.5678");
    Systom.out.println(bd.multiply(pd));
    ```

    `BigDecimal` 可以使用 `scale()` 方法来返回小数位数 且不忽略0

    ```java
    BigDecimal d1 = new BigDecimal("123.456");
    BigDecimal d2 = new BigDecimal("123.0000");
    System.out.println(d1.scale()); // 2
    System.out.println(d2.scale()); // 4
    ```

    如果想要忽略0， 那么可以使用`stripTrailingZeros()`方法，返回的也是一个BigDecimal

    ```java
    BigDecimal d1 = new BigDecimal("123.0000");
    BigDecimal d2 = d1.stripTrailingZeros();
    ```

    一个例子

    ```java
    import java.math.BigDecimal;
    import java.math.BigInteger;
    public class Main{
        public static void main(String[] args) {
            BigDecimal b1 = new BigDecimal("12345600");
            BigDecimal b2 = b1.stripTrailingZeros();
            System.out.println(b1.scale()); // 0
            System.out.println(b2.scale()); // -2
            System.out.println(b1); // 12345600
            System.out.println(b2); // 1.23456E+7
        }
    }
    ```

    说明 直接创造的b1 与经过 `stripTrailingZreos()` 创造的b2是不一样的。经过stripTrailingZeros() 的BigDecimal使用scale() 方法时可能会返回负数。负数的位数就是这个整数后面有几个0。

    可以对BigDecimal进行scale设置 `setScale()` 提供了这种方法.

    ```java
    public class Main{
      public static void main(String[] args){
        BigDecimal d1 = new BigDecimal("123.456789");
        BigDecimal d2 = d1.setScale(4, RoundingMode.HALF_UP); //四舍五入 123.4568
        BigDecimal d3 = d1.setScale(4, RoundingMode.DOWN) //直接截断 123.4567
      }
    }
    ```

    对BigDecimal做加，减，乘的时候，精度不会丢失，但是做除法时，有雨可能出现无法除尽的情况，所以要指定精度:

    ```java
    BigDecimal d1 = new BigDecimal("123.456");
    BigDecimal d2 = new BigDecimal("23.45689");
    BigDecimal d3 = d1.divide(d2, 10, RoundingMode.HALF_UP); // d2/d1 保留10位 四舍五入
    BigDecimal d4 = d1.divide(d2); // 报错 除不尽
    ```

    如果想要模除 得到余数和商 那么可以这样写

    ```java
    public class Main{
      public static void main(String[] args){
        BigDecimal n = new BigDecimal("12.345");
        BigDecimal m = new BigDecimal("0.12");
        BigDecimal[] dr = n.divideAndRemainder(m);
        System.out.println(dr[0]); //商
        System.out.println(dr[1]); //余数
      }
    }
    ```

    这样 如果想判断一个BigDecimal是不是另一个的整数倍 可以

    ```java
    import java.math.BigDecimal;
    public class Main{
        public static void main(String[] args) {
            BigDecimal b1 = new BigDecimal("12.75");
            BigDecimal b2 = new BigDecimal("0.15");
            BigDecimal[] dr = b1.divideAndRemainder(b2);
            System.out.println(dr[1]);
            System.out.println(dr[1].signum());
        }
    }
    ```

    其中 `signum()`方法是符号函数 正数1 负数-1 0为0

    不要用`equals()`方法去比较两个BigDecimal 因为scale不同 但数值相同的情况会被判断为false

    这是因为 BigDecimal这个类的字段有两个 一个是BigInteger intVal 一个是scale

    只有两者都相同的时候 才会返回true

    比较两个BigDecimal的正确方法是使用`compareTo()`

    这个方法 两者相同时会返回0 两者不同时会返回-1

21. 常用工具类

    - Math

      Math.abs()

      Math.max(a, b)

      Math.min(a, b)

      Math.pow(a, b)

      Math.sqrt(a);

      Math.exp(a);

      Math.log10(100);

      Math.sin/cos/tan/asin/acos(a);

      double pi = Math.PI;

      double e = Math.E;

      Math.random(); //生成一个 0 <= x < 1的随机数

      如果想要生成一个范围在 [min, max)的随机数 可以这样写

      ```java
      public class Main{
        public static void main(String[] args){
          double x = Math.random();
          double min = 10;
          double max = 50;
          double y = x*(max - min) + min;
          long n = (long) y;
          System.out.println(y);  // y是随机小数
          System.out.println(n);  // n是随机整数
        } 
      }
      ```

      Java标准库还提供了一个StrickMath 是为了保证跨平台性的 速度慢 一般用Math就行了

    - Random

      可以使用Random；来创造伪随机数，也就是读取随机数表 这样就能解释`next`的意思了

      Random r = new Random(seed); //如果不给种子 就默认为时间

      r.nextInt(); 生成一个整数随机数

      r.nextInt(10); 生成一个0-10之间的int

      r.nextLong(); 生成一个long随机数

      r.nextFloat(); 生成一个[0, 1)之间的float

      r.nextDouble(); 生成一个[0, 1)之间的double

    - SecureRandom

      随机数有真伪之分。真正的随机数是通过量子力学获取的，但如果你只是想要获取一个不可预测的随机数的话，用SecureRandom足够。

      `SecureRandom`无法指定种子 而是使用RNG(Random Number Generator)生成的，不同JDK的底层逻辑不一样

      ```java
      SecureRandom sr = new SecureRandom();
      System.out.println(sr.nextInt());
      ```

      jdk的rng有多种底层实现 实际使用时 可以优先获取高强度的rng，如果没有提供的话，再使用普通等级的tng

      ```java
      import java.util.Arrays;
      import java.security.SecureRandom;
      import java.security.NoSuchAlgorithmException;
      public class Main {
          public static void main(String[] args) {
              SecureRandom sr = null;
              try {
                  sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
              } catch (NoSuchAlgorithmException e) {
                  sr = new SecureRandom(); // 获取普通的安全随机数生成器
              }
              byte[] buffer = new byte[16];
              sr.nextBytes(buffer); // 用安全随机数填充buffer
              System.out.println(Arrays.toString(buffer));
          }
      }
      ```

      

      

      
