# TCP协议

## 1、<span style="color:brown">基础内容：</span>

**1.1、概述：**

TCP协议能够实现两台计算机之间的数据传输，但是传输的两端严格划分为Cilent、Server。

**1.2、两端通信的步骤：**

1. 服务端程序，需要提前打开，等待客户端连接；
2. 客户端要主动连接服务器，成功连接之后才能传输数据；
3. 服务器不会主动连接客户端；

**1.3、实现TCP通信的类：**

1. 客户端：java.net.Socket类

   ```apl
   1.创建Socket对象，向服务器发送连接请求;
   2.服务器响应请求，二者建立连接;
   ```

   

2. 服务器端：java.net.ServerSocket类
   ```apl
   1.创建ServerSocket对象，相当于开启一个服务;
   2.服务开启后，等待客户端连接;
   ```

**1.4、服务器端的运行：**

1. 多个客户端可以与服务器进行交互。因此，服务器要明确与哪个客户端进行交互，而服务器有一个accept方法，可以获取客户端请求对象；

2. 多个客户端与服务器交互，需要使用多个IO流对象。

   `服务器使用客户端的流跟客户端进行交互；`

## 2、<span style="color:brown">Socket类：</span>

**2.1、概述：**

1. `该类实现客户端套接字`；
2. <span style="color:orange">套接字：两台设备之间的通信的端点。包含了IP地址和端口号的网络单位</span>；

**2.2、构造方法：**

```java
public Socket(String host, int port)
    创建一个流套接字，并将其连接到指定的服务器上，且指定端口号
```

String host：服务器主机的名称  /  服务器的IP地址

int port：服务器的端口号

**2.3、成员方法：**

```java
public OutputStream getOuputStream()
    返回套接字的输出流;
```

```java
public InputStream getInputStream()
    返回套接字的输入流;
```

```java
public void shutdownInput()
	关闭输入流;
```

```java
public void shutdownOutput()
    关闭输出流;
```

```java
public void close()
    关闭套接字流;
```

## 3、<span style="color:brown">ServerSocket类：</span>

**3.1、概述：**

`此类实现服务器套接字`；

**3.2、构造方法：**

```java
public ServerSocket(int port)
    创建绑定到"指定端口号的服务器套接字";
```

**3.3、成员方法：**

```java
public Socket accept()
    侦听并接收到此服务器套接字的连接;
```

```java
public void close()
```

## 4、<span style="color:brown">客户端与服务端简单测试：</span>

````java
public class TCPServer {
    public static void main(String[] args) throws IOException {
        try {
            //1.创建服务器ServerSocket
            ServerSocket server = new ServerSocket(8080);
            System.out.println("服务器启动...");
            //2.调用accept方法进行监听，等待客户连接
            Socket act = server.accept();
            System.out.println("客户端："+act.getInetAddress().getHostAddress()+"已经连接到服务器");
            //3.获取输入流
            BufferedReader br = new BufferedReader(new InputStreamReader(act.getInputStream()));
            //4.读取客户端信息
            String mess = br.readLine();
            System.out.println("客户端："+mess);
            //5.关闭流
            act.shutdownInput();
            //6.获取输出流
            OutputStream out = act.getOutputStream();
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(out));
            //7.响应客户端的请求
            bw.write(mess);
            bw.newLine();
            bw.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
````

```java
/**
 * 如果服务器没有启动，就会抛出java.net.ConnectException异常
 * 如果服务器成功启动，就会连接成功
 */
public class TCPClient {
    public static void main(String[] args) throws IOException {
        try {
            //1.创建Socket通信
            Socket socket = new Socket("127.0.0.1", 8080);
            //2.获取输出流
            OutputStream out = socket.getOutputStream();
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(out));
            bw.write("测试客户端与服务器端通信,服务器收到客户端信息!");
            bw.newLine();
            bw.flush();
            socket.shutdownOutput();
            //3.获取输入流
            InputStream in = socket.getInputStream();
            //4.读取服务器响应信息
            BufferedReader br = new BufferedReader(new InputStreamReader(in));
            String mess = br.readLine();
            System.out.println("服务器："+mess);
            socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```apl
TCPServer:
	服务器启动...
	客户端：127.0.0.1已经连接到服务器
	客户端：测试客户端与服务器端通信，服务器收到客户端信息！
-----------------------------------------
TCPClient:
	服务器：测试客户端与服务器端通信，服务器收到客户端信息！
	Process finished with exit code 0
```

