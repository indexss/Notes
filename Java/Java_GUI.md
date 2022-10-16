# Java GUI

1. 核心技术 Swing AWT 不常用gui

   1. 因为不美观
   2. 需要jre环境

2. AWT是swing的原生 较为底层

   1. 介绍

      包含了很多的类和接口 GUI

      new 类

      很多元素： 窗口 按钮 文本框

   2. 组件和容器

      ![](https://s3.bmp.ovh/imgs/2022/10/14/0d0e5118ab98dd38.png)

      1. Frame

      ```java
      package com.indexss.lesson01;
      
      import java.awt.*;
      
      public class TestFrame {
          public static void main(String[] args) {
              Frame my_first_frame = new Frame("my first frame");
              //设置可见性
              my_first_frame.setVisible(true);
              //设置窗口大小
              my_first_frame.setSize(400,400);
              //设置颜色
              my_first_frame.setBackground(new Color(106, 37, 37));
              //弹出初始位置
              my_first_frame.setLocation(700,300);
              //设置是否可拉伸
              my_first_frame.setResizable(false);
          }
      }
      ```

      问题 发现窗口无法关闭。

      下面进行封装，创建多个窗口

      ```java
      package com.indexss.lesson01;
      
      import java.awt.*;
      
      public class TestFrame2 {
          public static void main(String[] args) {
              myFrame myFrame1 = new myFrame(100, 100, 200, 200, Color.BLUE);
              myFrame myFrame2 = new myFrame(305, 100, 200, 200, Color.PINK);
              myFrame myFrame3 = new myFrame(510, 100, 200, 200, Color.YELLOW);
              myFrame myFrame4 = new myFrame(715, 100, 200, 200, Color.GREEN);
      
          }
      }
      
      class myFrame extends Frame{
          static int id = 0;
      
          public myFrame(int x, int y, int w, int h, Color color){
              super("myFrame" + (++id));
              setBackground(color);
              setVisible(true);
              setBounds(x, y, w, h);
          }
      }
      
      ```

      2. 面板Panel

         可以将Panel堪称一个空间，但是不能单独存在，而是要放在Frame上

         解决关闭问题

         ```java
         package com.indexss.lesson01;
         
         import java.awt.*;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         import java.awt.event.WindowListener;
         
         public class TestPanel {
             public static void main(String[] args) {
                 Frame frame = new Frame();
                 //布局概念
                 Panel panel = new Panel();
                 //设置布局
                 frame.setLayout(null);
                 //坐标
                 frame.setBounds(300, 300, 500, 500);
                 frame.setBackground(new Color(96, 245, 131, 255));
                 //panel设置坐标 会相对于frame
                 panel.setBounds(50, 90, 400, 400);
                 panel.setBackground(new Color(197, 37, 37));
                 //frame.add(panel)
                 frame.add(panel);
                 frame.setVisible(true);
                 //监听事件，监听窗口关闭事件
                 //适配器模式
                 frame.addWindowListener(new WindowAdapter() {
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
             }
         }
         ```

      3. 布局管理器

         - 流式布局 从左到右

           ```java
           package com.indexss.lesson01;
           
           import java.awt.*;
           import java.awt.event.WindowAdapter;
           import java.awt.event.WindowEvent;
           import java.util.concurrent.Flow;
           
           public class TestFlowLayout {
               public static void main(String[] args) {
                   Frame frame = new Frame();
                   //组件 按钮
                   Button button1 = new Button("button1");
                   Button button2 = new Button("button2");
                   Button button3 = new Button("button3");
                   //设置为流式布局
           //        frame.setLayout(new FlowLayout());
                   frame.setLayout(new FlowLayout(FlowLayout.LEFT));
                   frame.setBounds(200, 200 ,400, 400);
                   //把按钮添加上去
                   frame.add(button1);
                   frame.add(button2);
                   frame.add(button3);
                   frame.setVisible(true);
                   frame.addWindowListener(new WindowAdapter() {
                       @Override
                       public void windowClosing(WindowEvent e) {
                           System.exit(0);
                       }
                   });
               }
           }
           ```

         - 东西南北中

           ```java
           package com.indexss.lesson01;
           
           import java.awt.*;
           import java.awt.event.WindowAdapter;
           import java.awt.event.WindowEvent;
           
           public class TestBorderLayout {
               public static void main(String[] args) {
                   Frame testBodyLayout = new Frame("TestBodyLayout");
                   Button east = new Button("East");
                   Button west = new Button("West");
                   Button south = new Button("South");
                   Button north = new Button("North");
                   Button center = new Button("Center");
           
                   testBodyLayout.add(east, BorderLayout.EAST);
                   testBodyLayout.add(west, BorderLayout.WEST);
                   testBodyLayout.add(south, BorderLayout.SOUTH);
                   testBodyLayout.add(north, BorderLayout.NORTH);
                   testBodyLayout.add(center, BorderLayout.CENTER);
           
                   testBodyLayout.setBounds(200,200,400,400);
                   testBodyLayout.setVisible(true);
                   testBodyLayout.addWindowListener(new WindowAdapter() {
                       @Override
                       public void windowClosing(WindowEvent e) {
                           System.exit(0);
                       }
                   });
           
               }
           }
           
           ```

           

         - 表格布局 Grid

           ```java
           package com.indexss.lesson01;
           
           import java.awt.*;
           import java.awt.event.WindowAdapter;
           import java.awt.event.WindowEvent;
           
           public class TestGridLayout {
               public static void main(String[] args) {
                   Frame frame = new Frame();
                   Button button1 = new Button("btn1");
                   Button button2 = new Button("btn2");
                   Button button3 = new Button("btn3");
                   Button button4 = new Button("btn4");
                   Button button5 = new Button("btn5");
                   Button button6 = new Button("btn6");
           
                   frame.setLayout(new GridLayout(3, 2));
           
                   frame.add(button1);
                   frame.add(button2);
                   frame.add(button3);
                   frame.add(button4);
                   frame.add(button5);
                   frame.add(button6);
           //        frame.pack();  //按照最优布局！！！
                   frame.setBounds(200,200, 400, 400);
                   frame.addWindowListener(new WindowAdapter() {
                       @Override
                       public void windowClosing(WindowEvent e) {
                           System.exit(0);
                       }
                   });
                   frame.setVisible(true);
               }
           }
           
           ```

      4. 事件监听

         事件监听：当某个事件发生的时候，干什么？

         首先是基本的监听

         ```java
         package com.indexss.lesson02;
         
         import java.awt.*;
         import java.awt.event.ActionEvent;
         import java.awt.event.ActionListener;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         
         public class TestActionEvent {
             public static void main(String[] args) {
                 Frame frame = new Frame();
                 Button button = new Button();
                 frame.add(button, BorderLayout.CENTER);
                 frame.pack();
         //        MyActionLitsoner myActionLitsoner = new MyActionLitsoner();
                 button.addActionListener((ActionEvent e) -> System.out.println(conter.x++)); // 这里用的是lambda表达式
                 frame.setVisible(true);
         
                 frame.addWindowListener(new WindowAdapter() {
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
         
             }
         }
         class conter{
             static int x = 0;
         }
         
         // 事件监听 由于addActionListener需要一个实现ActionListener接口的类实例，所以在这里写一个
         //上面用了Lambda表达式，即只给输入和输出，不用管名字，实现接口，因为这些都可以推断出来
         //class MyActionLitsoner implements ActionListener {
         //    static int x = 0;
         //    @Override
         //    public void actionPerformed(ActionEvent e){
         //        System.out.println("the" + x++);
         //    }
         //}
         ```

         然后是两个按钮的监听 监听到后做相同的事情

         ```java
         package com.indexss.lesson02;
         
         import java.awt.*;
         import java.awt.event.ActionEvent;
         import java.awt.event.ActionListener;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         
         public class TestActionEvent02 {
             public static void main(String[] args) {
                 Frame frame = new Frame();
                 frame.setBounds(200,200,400,400);
                 frame.setLayout(new GridLayout(2,1));
         
                 Button button1 = new Button("开始");
                 button1.setActionCommand("1 command");
         
                 Button button2 = new Button("结束");
                 button2.setActionCommand("2 command");
         
                 frame.add(button1);
                 frame.add(button2);
         
                 myImplement myImplement = new myImplement();
                 button1.addActionListener(myImplement);
                 button2.addActionListener(myImplement);
         
                 frame.addWindowListener(new WindowAdapter() {
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
                 frame.setVisible(true);
             }
         }
         
         class myImplement implements ActionListener{
             public void actionPerformed(ActionEvent e){
                 System.out.println(e.getActionCommand());
                 if(e.getActionCommand().equals("1 command")){
                     System.out.println("第一条指令被判断了");
                 }else{
                     System.out.println("第二条指令被判断了");
                 }
             }
         }
         
         ```

      5. 输入框TextField监听 

         ```java
         package com.indexss.lesson02;
         
         import java.awt.*;
         import java.awt.event.ActionEvent;
         import java.awt.event.ActionListener;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         
         public class TestText01 {
             public static void main(String[] args) {
                 MyFrameStart myFrameStart = new MyFrameStart(200, 200, 400, 400);
             }
         }
         
         class MyFrameStart extends Frame {
             public MyFrameStart(int x, int y, int w, int h){
                 super();
                 this.setBounds(x, y, w, h);
         
                 this.addWindowListener(new WindowAdapter() {
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
         
                 TextField textField = new TextField();
                 this.add(textField);
         
                 //监听文本框 按下回车就会触发输入框的事件
                 myListener myListener = new myListener();
                 textField.addActionListener(myListener);
         
                 //设置显示的替换文本
                 textField.setEchoChar('*');
                 this.setVisible(true);
             }
         }
         
         class myListener implements ActionListener{
             public void actionPerformed(ActionEvent e){
                 TextField txt = (TextField) e.getSource(); //向下转型 可以使用txt的方法
                 String str = txt.getText();
                 System.out.println(str);
                 txt.setText("");
             }
         }
         ```

      6. 简易计算器

         oop原则 组合大于继承

         ```java
         class A extends B{ //继承
         }
         ```

         组合是这个样子的

         ```java
         class A{
           public B b;
           public C c;
         }
         ```

         第一版的简易计算器的代码是这样的

         ```java
         package com.indexss.lesson02;
         
         import org.w3c.dom.Text;
         
         import javax.swing.*;
         import java.awt.*;
         import java.awt.event.ActionEvent;
         import java.awt.event.ActionListener;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         
         //main
         public class TestCalc {
             public static void main(String[] args) {
                 Calculator calculator = new Calculator(200, 200, 400, 400);
             }
         }
         
         //计算器类
         class Calculator extends Frame{
             public Calculator(int x, int y, int w, int h){
         
                 //重复性工作
                 super();
                 this.setBounds(x, y, w, h);
         
                 this.addWindowListener(new WindowAdapter() {
         
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
         
                 //三个文本框
                 TextField textField1 = new TextField(10); //最多能填10个数字
                 TextField textField2 = new TextField(10); //最多能填10个数字
                 TextField textField3 = new TextField(20); //最多能填10个数字
         
                 //一个按钮
                 Button button = new Button("=");
                 button.addActionListener(new myActionListener(textField1, textField2, textField3));
         
                 //一个标签
                 Label label = new Label("+");
         
                 //开始布局
                 this.setLayout(new FlowLayout());
                 this.add(textField1);
                 this.add(label);
                 this.add(textField2);
                 this.add(button);
                 this.add(textField3);
                 this.pack();
                 this.setVisible(true);
             }
         }
         
         //监听器类
         class myActionListener implements ActionListener{
             //获取三个变量
             private TextField num1;
             private TextField num2;
             private TextField num3;
             public myActionListener(TextField textField1, TextField textField2, TextField textField3){
                 this.num1 = textField1;
                 this.num2 = textField2;
                 this.num3 = textField3;
             }
         
             @Override
             public void actionPerformed(ActionEvent e) {
         
                 //1. 获得两个加数
                 double n1 = Double.parseDouble(num1.getText());
                 double n2 = Double.parseDouble(num2.getText());
         
                 //2. 将结果放到第三个框
                 double n3 = n1 + n2;
                 num3.setText("" + n3);
         
                 //3. 清空前两个框
                 num1.setText("");
                 num2.setText("");
             }
         }
         ```

         当我们使用了「组合」的方式去解决监听器传入过多参数的问题时，代码是这样的

         ```java
         package com.indexss.lesson02;
         import java.awt.*;
         import java.awt.event.ActionEvent;
         import java.awt.event.ActionListener;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         
         public class cal{
             public static void main(String[] args) {
                 new Calculator().loadFrame();
             }
         }
         
         class Calculator extends Frame {
             // 属性
             TextField num1, num2, num3;
             // 方法
             public void loadFrame() {
                 //新建组件
                 num1 = new TextField(10);
                 num2 = new TextField(10);
                 num3 = new TextField(10);
                 Button button = new Button("=");
                 button.addActionListener(new MyCalculatorListener(this));
                 Label label = new Label();
                 
                 //布局
                 setLayout(new FlowLayout());
                 add(num1);
                 add(label);
                 add(num2);
                 add(button);
                 add(num3);
                 
                 //经典操作
                 addWindowListener(new WindowAdapter() {
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
                 this.pack();
                 setVisible(true);
             }
         
             class MyCalculatorListener implements ActionListener {
                 //类的组合：不直接传入参数，而是传入封装的对象
                 Calculator calculator = null;
         
                 public MyCalculatorListener(Calculator calculator) {
                     this.calculator = calculator;
                 }
         
                 @Override
                 public void actionPerformed(ActionEvent e) {
                     int n1 = Integer.parseInt(calculator.num1.getText());
                     int n2 = Integer.parseInt(calculator.num2.getText());
                     int result = n1 + n2;
                     System.out.println();
                     calculator.num3.setText("" + result);
                     calculator.num1.setText("");
                     calculator.num2.setText("");
                 }
             }
         }
         
         ```

         改动的方法就是，将我们直接要使用过的num1, num2, num3做成属性字段，然后再监听器汇总创造一个`Calculator`类的引用，然后通过调用Calculator来获得想要的num1,2,3.

         当然，我们还可以使用「内部类」的方法去解决问题，这样过程中就有了更好的包装

         ```java
         package com.indexss.lesson02;
         import java.awt.*;
         import java.awt.event.ActionEvent;
         import java.awt.event.ActionListener;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         
         public class cal{
             public static void main(String[] args) {
                 new Calculator().loadFrame();
             }
         }
         
         class Calculator extends Frame {
             // 属性
             TextField num1, num2, num3;
             // 方法
             public void loadFrame() {
                 //新建组件
                 num1 = new TextField(10);
                 num2 = new TextField(10);
                 num3 = new TextField(10);
                 Button button = new Button("=");
                 button.addActionListener(new MyCalculatorListener());
                 Label label = new Label();
         
                 //布局
                 setLayout(new FlowLayout());
                 add(num1);
                 add(label);
                 add(num2);
                 add(button);
                 add(num3);
         
                 //经典操作
                 addWindowListener(new WindowAdapter() {
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
                 this.pack();
                 setVisible(true);
             }
         
             private  class MyCalculatorListener implements ActionListener {
                 @Override
                 public void actionPerformed(ActionEvent e) {
                     int n1 = Integer.parseInt(num1.getText());
                     int n2 = Integer.parseInt(num2.getText());
                     int result = n1 + n2;
                     System.out.println();
                     num3.setText("" + result);
                     num1.setText("");
                     num2.setText("");
                 }
             }
         
         }
         
         ```

         内部类是一个简化代码的过程。由于我们直接把MyCalculatorListener写到了Calculator这个类的里面，所以我们不需要再去写一个构造函数去获得calculator这个实例，因为内部类可以调用外部类的字段。在addActionListener的时候，我们可以直接拿到内部类。

         我个人感觉这种方式虽然简洁，但是你需要对于作用域的概念十分清楚。而且因为写成了内部类的形式，我们不再能够将监听器单独写成一个类，这样也增加了我们维护代码的难度。所以我本人不会大面积地使用这种「阿里巴巴手册」中非常推崇的代码风格。

         当然，这只是我个人的看法捏0.0

         我还用lambda表达式去重写了一下监听器，样子是这样的

         ```java
         package com.indexss.lesson02;
         import java.awt.*;
         import java.awt.event.ActionEvent;
         import java.awt.event.ActionListener;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         
         public class cal{
             public static void main(String[] args) {
                 new Calculator().loadFrame();
             }
         }
         
         class Calculator extends Frame {
             // 属性
             TextField num1, num2, num3;
             // 方法
             public void loadFrame() {
                 //新建组件
                 num1 = new TextField(10);
                 num2 = new TextField(10);
                 num3 = new TextField(10);
                 Button button = new Button("=");
                 button.addActionListener((ActionEvent e)->{
                     num3.setText(Integer.parseInt(num1.getText()) + Integer.parseInt(num2.getText())+"");
                     num1.setText("");
                     num2.setText("");
                 });
                 Label label = new Label();
         
                 //布局
                 setLayout(new FlowLayout());
                 add(num1);
                 add(label);
                 add(num2);
                 add(button);
                 add(num3);
         
                 //经典操作
                 addWindowListener(new WindowAdapter() {
                     @Override
                     public void windowClosing(WindowEvent e) {
                         System.exit(0);
                     }
                 });
                 this.pack();
                 setVisible(true);
             }
         }
         
         ```

         反正就是越来越简洁，可读性越来越低

      7. 画笔paint

         ```java
         package com.indexss;
         
         import java.awt.*;
         
         public class TestPaint {
             public static void main(String[] args) {
                 Paint paint = new Paint();
                 paint.loadFrame();
             }
         }
         
         class Paint extends Frame {
         
             public void loadFrame(){
                 setBounds(200,200,500,600);
                 setVisible(true);
             }
         
             @Override
             public void paint(Graphics g) {
                 //画笔，颜色，画的内容
                 g.setColor(Color.green);
                 g.drawOval(100,100,100,100);
                 g.setColor(Color.BLUE);
                 g.fillRect(200,200,100,100);
         
                 //画笔用完 把它还原成最初的颜色
                 
             }
         }
         ```

      8. 鼠标监听

         目的：监听鼠标点击坐标

         ```java
         package com.indexss;
         
         import java.awt.*;
         import java.awt.event.MouseAdapter;
         import java.awt.event.MouseEvent;
         import java.awt.event.MouseListener;
         import java.util.ArrayList;
         import java.util.Iterator;
         
         //鼠标监听事件
         public class TestMouseListener {
             public static void main(String[] args) {
                 MyFrame myFrame = new MyFrame("myFrame");
             }
         }
         
         class MyFrame extends Frame {
             ArrayList points;
             //画画需要画笔，需要监听鼠标当前的位置，需要集合来存储这个点
             public MyFrame(String title){
                 super(title);
                 setBounds(200, 200, 400, 300);
                 //存鼠标点击的点
                 points = new ArrayList<>();
                 //鼠标监听器，针对窗口来说的监听器，因为你不监听外面的世界
                 this.addMouseListener(new MyMouseListener());
         
                 setVisible(true);
             }
         
             @Override
             public void paint(Graphics g) {
                 //画画 需要监听鼠标的事件
                 Iterator iterator = points.iterator();
                 while(iterator.hasNext()){  //这两行可以不断发掘points类里面有没有新的对象，不会结束
                     Point point = (Point) iterator.next();
                     g.setColor(Color.CYAN);
                     g.fillOval(point.x, point.y, 10, 10);
                 }
             }
         
             public void addPoint(Point point){
                 points.add(point);
             }
         
             private class MyMouseListener extends MouseAdapter {
                 //鼠标点击：按下，弹起，按住不放
         
                 @Override
                 public void mousePressed(MouseEvent e) {
                     MyFrame myFrame = (MyFrame) e.getSource();
                     //这里的getSource()返回的东西是调用，添加监听器的对象
                     //我们的监听器显然是由MyFrame中的this创建的，所以说我们要让e.getSource()返回的Object类向下转型成为MyFrame类
                     //这里点击的时候，界面上就会产生一个点
                     //这个点就是鼠标的点
                     myFrame.addPoint(new Point(e.getX(), e.getY()));
                     //每次点击鼠标都需要重新画一遍
                     //我们的过程是 先执行paint(执行while循环 迭代器探测有没有点)，然后获取鼠标点击坐标，然后将鼠标点击坐标实例化成一个点，然后加入到点集中。但是由于Frame类默认只会执行Paint一次，所以我们如果不加下面的repaint的话就画不出点。而由于现在所处的类是鼠标监听器，我们的repaint会被执行很多遍，所以可以话出多个点。
                     //这个问题的关键就在于 Frame创建出来的时候，就一定会第一时间执行一次paint，所以我们第一次是绝对画不上点的。
                     myFrame.repaint();
         
                 }
             }
         }
         ```

      9. 窗口监听

         ```java
         package com.indexss;
         
         import java.awt.*;
         import java.awt.event.WindowAdapter;
         import java.awt.event.WindowEvent;
         import java.awt.event.WindowListener;
         
         public class TestWindow {
             public static void main(String[] args) {
                 new WindowFrame();
             }
         }
         
         class WindowFrame extends Frame{
             public WindowFrame(){
                 setBounds(200,200,500,500);
                 setBackground(Color.BLUE);
                 addWindowListener(new MyWindowListener());
                 setVisible(true);
         
             }
         
             class MyWindowListener extends WindowAdapter {
         
                 //关闭窗口事件
                 @Override
                 public void windowClosing(WindowEvent e) {
                     System.out.println("Closing");
                     //隐藏窗口
                     setVisible(false); //通过按钮隐藏窗口
                     //关闭窗口
                     System.exit(0);
                 }
         
                 //窗口打开事件
                 @Override
                 public void windowOpened(WindowEvent e) {
                     System.out.println("Open");
                 }
         
                 //激活窗口事件（光标聚焦）
                 @Override
                 public void windowActivated(WindowEvent e) {
                     System.out.println("Activate");
                 }
         
                 //已经关闭
                 //这个由于是结束进程的关闭，所以监听不到
                 @Override
                 public void windowClosed(WindowEvent e) {
                     System.out.println("closed");
                 }
         
                 //失去焦点
                 //这个也监听不到
                 @Override
                 public void windowLostFocus(WindowEvent e) {
                     System.out.println("lostFocus");
                 }
             }
         }
         ```

      10. 键盘监听

          ```java
          package com.indexss;
          
          import java.awt.*;
          import java.awt.event.KeyAdapter;
          import java.awt.event.KeyEvent;
          
          public class TestKeyListener {
              public static void main(String[] args) {
                  new KeyFrame();
              }
          }
          
          class KeyFrame extends Frame {
              public KeyFrame(){
                  setBounds(200, 200 ,400, 400);
                  setVisible(true);
          
                  this.addKeyListener(new KeyAdapter() {
                      @Override
                      public void keyPressed(KeyEvent e) {
                          //获得当前键盘的码
                          int keyCode = e.getKeyCode();
                          System.out.println("你按下了"+keyCode);
                      }
                  });
              }
          }
          
          ```

          

3. Swing

   

​		

