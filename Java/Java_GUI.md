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

         

         

3. Swing

​		

