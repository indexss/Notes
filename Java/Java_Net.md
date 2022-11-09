# Java网络编程

1. 一个简单的C/S交互实例

   AcceptTest_Server

   ```java
   package com.server;
   
   import java.net.ServerSocket;
   import java.net.Socket;
   
   public class AcceptTest_Server {
       public static void main(String[] args) throws Exception{
           ServerSocket serverSocket = new ServerSocket(9999);
           System.out.println("未连接");
           Socket socket = serverSocket.accept();
           System.out.println("连接");
       }
   }
   
   ```

   AcceptTest_Client

   ```java
   package com.server;
   
   import java.net.Socket;
   
   public class AcceptTest_Client {
       public static void main(String[] args) throws Exception{
           Socket socket = new Socket("127.0.0.1", 9999);
       }
   }
   
   ```

2. 另一个简单的例子

   Server

   ```java
   package com.chat1;
   
   import java.net.ServerSocket;
   import java.net.Socket;
   
   public class server1 {
       public ServerSocket serversocket1;
       public Socket socket1;
   
       public server1() throws Exception{
           System.out.println("未连接");
           serversocket1 = new ServerSocket(12000);
           socket1 = serversocket1.accept();
           String clientIPAddress = socket1.getInetAddress().getHostAddress();
           System.out.println(clientIPAddress + "已连接");
       }
   
       public static void main(String[] args) throws Exception{
           new server1();
       }
   }
   
   ```

   Client

   ```java
   package com.chat1;
   
   import java.net.Socket;
   
   public class client1 {
       public Socket socket2;
   
       public client1() throws Exception{
           socket2 = new Socket("127.0.0.1", 12000);
           System.out.println("您已经连接");
       }
   
       public static void main(String[] args) throws Exception {
           new client1();
       }
   }
   
   ```

   

2. tcp网络聊天室的实现

   `Server`

   ```java
   package com.chat5;
   
   import javax.swing.*;
   import java.awt.*;
   import java.io.BufferedReader;
   import java.io.InputStream;
   import java.io.InputStreamReader;
   import java.io.PrintStream;
   import java.net.ServerSocket;
   import java.net.Socket;
   import java.util.ArrayList;
   
   public class Server extends JFrame implements Runnable {
   
       private Socket s = null;
       private ServerSocket ss = null;
       private ArrayList<ChatThread> clients = new ArrayList();
   
       public Server() throws Exception {
           setTitle("服务器");
           setDefaultCloseOperation(EXIT_ON_CLOSE);
           this.setBackground(Color.BLUE);
           setSize(200, 100);
           setVisible(true);
           ss = new ServerSocket(9999);
           new Thread(this).start();
       }
   
       @Override
       public void run() {
           try {
               while (true) {
                   s = ss.accept(); //接线
                   ChatThread ct = new ChatThread(s);
                   clients.add(ct);
                   ct.start();
               }
           } catch (Exception ex) {
               ex.printStackTrace();
           }
       }
   
       class ChatThread extends Thread {
           private Socket s = null;
           private BufferedReader br = null;
           public PrintStream ps = null;
   
           public ChatThread(Socket s) throws Exception {
               this.s = s;
               br = new BufferedReader(new InputStreamReader(s.getInputStream())); //读的
               ps = new PrintStream(s.getOutputStream());  //写的
           }
   
           @Override
           public void run() { //看看这个ChatThread线程到底干了点什么事
               try {
                   while (true) {
                       String str = br.readLine();
   //                    sendMessage(str);  //将sendMessage(str)展开，如下所示
                       for (int i = 0; i < clients.size(); i++) {
                           ChatThread ct = clients.get(i);
                           ct.ps.println(str);
                       }
                   }
               } catch (Exception e100) {
                   e100.printStackTrace();
               }
           }
       }
   //
   //    public void sendMessage(String msg) {
   //        for (int i = 0; i < clients.size(); i++) {
   //            ChatThread ct = (ChatThread) clients.get(i);
   //            ct.ps.println(msg);
   //        }
   //    }
   
       public static void main(String[] args) throws Exception {
           Server server = new Server();
       }
   }
   
   ```

   `Client`

   ```java
   package com.chat5;
   
   import javax.swing.*;
   import java.awt.*;
   import java.awt.event.ActionEvent;
   import java.awt.event.ActionListener;
   import java.io.*;
   import java.net.Socket;
   
   public class Client extends JFrame implements ActionListener, Runnable {
   
       private JTextArea taMsg = new JTextArea("以下是聊天内容：\n ");
       private JTextField tfMsg = new JTextField();
       private Socket s = null;
       private String nickName = null;
   
       public Client() {
           setTitle("客户端");
           setDefaultCloseOperation(EXIT_ON_CLOSE);
           this.add(taMsg, BorderLayout.CENTER); //ta是自己这里显示的文字
           tfMsg.setBackground(Color.YELLOW);
           this.add(tfMsg, BorderLayout.SOUTH); //tf是给别人的文字
           tfMsg.addActionListener(this);
           this.setSize(280, 400);
           setVisible(true);
           nickName = JOptionPane.showInputDialog("输入昵称");
   
           try {
               s = new Socket("127.0.0.1", 9999);
               JOptionPane.showMessageDialog(this, "连接成功");
               this.setTitle("客户端" + nickName);
               new Thread(this).start();
           } catch (Exception ex123) {
               ex123.printStackTrace();
           }
       }
   
       @Override
       public void actionPerformed(ActionEvent e) {
           try {
               OutputStream os = s.getOutputStream();
               PrintStream ps = new PrintStream(os);
               ps.println(nickName + "说: " + tfMsg.getText());
               tfMsg.setText("");
           } catch (Exception ex101) {
               ex101.printStackTrace();
           }
       }
   
       @Override
       public void run() {
           try {
               while (true) {
                   InputStream is = s.getInputStream();
                   BufferedReader br = new BufferedReader(new InputStreamReader(is));
                   String str = br.readLine();
                   taMsg.append(str + "\n");
               }
           } catch (Exception ex100) {
               ex100.printStackTrace();
           }
       }
   
       public static void main(String[] args) throws Exception {
           Client client = new Client();
       }
   }
   
   ```

   
