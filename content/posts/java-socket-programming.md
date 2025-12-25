---
title: "Lập trình Socket với Java - TCP/IP Communication"
date: 2025-12-16
draft: false
tags: ["Java", "Socket", "Network Programming", "TCP/IP"]
categories: ["Java"]
---

# Lập trình Socket với Java

Socket programming là nền tảng của lập trình mạng, cho phép các ứng dụng giao tiếp qua mạng sử dụng giao thức TCP/IP.

## Socket là gì?

Socket là endpoint của kết nối hai chiều giữa hai chương trình chạy trên mạng. Java cung cấp hai loại socket chính:
- **ServerSocket**: Lắng nghe kết nối từ client
- **Socket**: Kết nối đến server

## TCP Server - Echo Server

Server nhận tin nhắn từ client và gửi lại (echo).

```java
import java.io.*;
import java.net.*;

public class EchoServer {
    private ServerSocket serverSocket;
    
    public void start(int port) {
        try {
            serverSocket = new ServerSocket(port);
            System.out.println("Server đang lắng nghe tại port " + port);
            
            while (true) {
                // Chấp nhận kết nối từ client
                Socket clientSocket = serverSocket.accept();
                System.out.println("Client kết nối: " + clientSocket.getInetAddress());
                
                // Xử lý client trong thread riêng
                new Thread(new ClientHandler(clientSocket)).start();
            }
        } catch (IOException e) {
            System.out.println("Lỗi server: " + e.getMessage());
        }
    }
    
    public void stop() {
        try {
            if (serverSocket != null) {
                serverSocket.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    // Handler cho mỗi client
    private class ClientHandler implements Runnable {
        private Socket clientSocket;
        
        public ClientHandler(Socket socket) {
            this.clientSocket = socket;
        }
        
        @Override
        public void run() {
            try (
                BufferedReader in = new BufferedReader(
                    new InputStreamReader(clientSocket.getInputStream()));
                PrintWriter out = new PrintWriter(
                    clientSocket.getOutputStream(), true)
            ) {
                String inputLine;
                
                // Đọc và echo lại tin nhắn
                while ((inputLine = in.readLine()) != null) {
                    System.out.println("Nhận: " + inputLine);
                    
                    if ("bye".equalsIgnoreCase(inputLine)) {
                        out.println("Goodbye!");
                        break;
                    }
                    
                    // Echo lại tin nhắn
                    out.println("Echo: " + inputLine);
                }
                
                clientSocket.close();
                System.out.println("Client ngắt kết nối");
                
            } catch (IOException e) {
                System.out.println("Lỗi xử lý client: " + e.getMessage());
            }
        }
    }
    
    public static void main(String[] args) {
        EchoServer server = new EchoServer();
        server.start(8080);
    }
}
```

## TCP Client - Kết nối đến Server

```java
import java.io.*;
import java.net.*;
import java.util.Scanner;

public class EchoClient {
    private Socket socket;
    private PrintWriter out;
    private BufferedReader in;
    
    public void connect(String host, int port) {
        try {
            socket = new Socket(host, port);
            System.out.println("Đã kết nối đến server " + host + ":" + port);
            
            out = new PrintWriter(socket.getOutputStream(), true);
            in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            
        } catch (UnknownHostException e) {
            System.out.println("Không tìm thấy host: " + e.getMessage());
        } catch (IOException e) {
            System.out.println("Lỗi kết nối: " + e.getMessage());
        }
    }
    
    public String sendMessage(String message) {
        try {
            out.println(message);
            return in.readLine();
        } catch (IOException e) {
            System.out.println("Lỗi gửi/nhận: " + e.getMessage());
            return null;
        }
    }
    
    public void disconnect() {
        try {
            if (in != null) in.close();
            if (out != null) out.close();
            if (socket != null) socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        EchoClient client = new EchoClient();
        client.connect("localhost", 8080);
        
        Scanner scanner = new Scanner(System.in);
        String message;
        
        System.out.println("Nhập tin nhắn (gõ 'bye' để thoát):");
        
        while (true) {
            System.out.print("> ");
            message = scanner.nextLine();
            
            String response = client.sendMessage(message);
            System.out.println("Server: " + response);
            
            if ("bye".equalsIgnoreCase(message)) {
                break;
            }
        }
        
        client.disconnect();
        scanner.close();
        System.out.println("Đã ngắt kết nối");
    }
}
```

## Multi-threaded Chat Server

Server chat hỗ trợ nhiều client đồng thời.

```java
import java.io.*;
import java.net.*;
import java.util.*;

public class ChatServer {
    private static final int PORT = 8080;
    private static Set<ClientHandler> clientHandlers = new HashSet<>();
    
    public static void main(String[] args) {
        System.out.println("Chat Server khởi động tại port " + PORT);
        
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            while (true) {
                Socket socket = serverSocket.accept();
                
                ClientHandler clientHandler = new ClientHandler(socket);
                clientHandlers.add(clientHandler);
                
                new Thread(clientHandler).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    // Broadcast tin nhắn đến tất cả clients
    public static void broadcast(String message, ClientHandler sender) {
        for (ClientHandler client : clientHandlers) {
            if (client != sender) {
                client.sendMessage(message);
            }
        }
    }
    
    // Xóa client khi ngắt kết nối
    public static void removeClient(ClientHandler client) {
        clientHandlers.remove(client);
    }
    
    private static class ClientHandler implements Runnable {
        private Socket socket;
        private PrintWriter out;
        private String username;
        
        public ClientHandler(Socket socket) {
            this.socket = socket;
        }
        
        @Override
        public void run() {
            try {
                BufferedReader in = new BufferedReader(
                    new InputStreamReader(socket.getInputStream()));
                out = new PrintWriter(socket.getOutputStream(), true);
                
                // Nhận username
                out.println("Nhập username của bạn:");
                username = in.readLine();
                
                System.out.println(username + " đã tham gia chat");
                broadcast(username + " đã tham gia chat!", this);
                
                // Nhận và broadcast tin nhắn
                String message;
                while ((message = in.readLine()) != null) {
                    if (message.equalsIgnoreCase("/quit")) {
                        break;
                    }
                    
                    System.out.println(username + ": " + message);
                    broadcast(username + ": " + message, this);
                }
                
            } catch (IOException e) {
                System.out.println("Lỗi: " + e.getMessage());
            } finally {
                disconnect();
            }
        }
        
        public void sendMessage(String message) {
            if (out != null) {
                out.println(message);
            }
        }
        
        private void disconnect() {
            try {
                if (socket != null) {
                    socket.close();
                }
                removeClient(this);
                broadcast(username + " đã rời khỏi chat!", this);
                System.out.println(username + " đã ngắt kết nối");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## Chat Client

```java
import java.io.*;
import java.net.*;
import java.util.Scanner;

public class ChatClient {
    private Socket socket;
    private PrintWriter out;
    private BufferedReader in;
    
    public void connect(String host, int port) {
        try {
            socket = new Socket(host, port);
            out = new PrintWriter(socket.getOutputStream(), true);
            in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            
            // Thread nhận tin nhắn từ server
            new Thread(new MessageReceiver()).start();
            
            // Thread gửi tin nhắn
            Scanner scanner = new Scanner(System.in);
            String message;
            
            while (true) {
                message = scanner.nextLine();
                out.println(message);
                
                if (message.equalsIgnoreCase("/quit")) {
                    break;
                }
            }
            
            scanner.close();
            disconnect();
            
        } catch (IOException e) {
            System.out.println("Lỗi kết nối: " + e.getMessage());
        }
    }
    
    private class MessageReceiver implements Runnable {
        @Override
        public void run() {
            try {
                String message;
                while ((message = in.readLine()) != null) {
                    System.out.println(message);
                }
            } catch (IOException e) {
                System.out.println("Ngắt kết nối từ server");
            }
        }
    }
    
    private void disconnect() {
        try {
            if (in != null) in.close();
            if (out != null) out.close();
            if (socket != null) socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        ChatClient client = new ChatClient();
        client.connect("localhost", 8080);
    }
}
```

## File Transfer qua Socket

```java
// Server nhận file
public class FileReceiver {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080)) {
            System.out.println("Đợi nhận file...");
            
            Socket socket = serverSocket.accept();
            
            // Nhận tên file
            BufferedReader reader = new BufferedReader(
                new InputStreamReader(socket.getInputStream()));
            String fileName = reader.readLine();
            
            // Nhận file
            FileOutputStream fos = new FileOutputStream("received_" + fileName);
            InputStream is = socket.getInputStream();
            
            byte[] buffer = new byte[4096];
            int bytesRead;
            
            while ((bytesRead = is.read(buffer)) != -1) {
                fos.write(buffer, 0, bytesRead);
            }
            
            fos.close();
            socket.close();
            
            System.out.println("File đã được nhận: received_" + fileName);
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// Client gửi file
public class FileSender {
    public static void main(String[] args) {
        String fileName = "test.txt";
        
        try (Socket socket = new Socket("localhost", 8080)) {
            
            // Gửi tên file
            PrintWriter writer = new PrintWriter(socket.getOutputStream(), true);
            writer.println(fileName);
            
            // Gửi file
            FileInputStream fis = new FileInputStream(fileName);
            OutputStream os = socket.getOutputStream();
            
            byte[] buffer = new byte[4096];
            int bytesRead;
            
            while ((bytesRead = fis.read(buffer)) != -1) {
                os.write(buffer, 0, bytesRead);
            }
            
            fis.close();
            socket.close();
            
            System.out.println("File đã được gửi: " + fileName);
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Best Practices

1. **Luôn đóng resources**: Sử dụng try-with-resources
2. **Xử lý exceptions**: Bắt và xử lý IOException đúng cách
3. **Multi-threading**: Sử dụng thread cho mỗi client
4. **Buffer size**: Chọn buffer size phù hợp (4KB - 8KB)
5. **Timeout**: Set timeout để tránh blocking vô hạn

```java
socket.setSoTimeout(5000); // 5 seconds timeout
```

## Kết luận

Socket programming là nền tảng cho:
- Chat applications
- File transfer
- Game multiplayer
- Client-server applications

Hãy thực hành để làm chủ network programming! 🌐
