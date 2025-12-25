---
title: "Lập trình đa luồng (Multithreading) trong Java"
date: 2025-12-17
draft: false
tags: ["Java", "Multithreading", "Concurrency"]
categories: ["Java"]
---

# Lập trình đa luồng trong Java

Multithreading cho phép chương trình thực thi nhiều tác vụ đồng thời, tận dụng tối đa sức mạnh của CPU.

## Thread là gì?

Thread (luồng) là đơn vị nhỏ nhất của một process. Một chương trình có thể có nhiều thread chạy song song.

## Tạo Thread trong Java

### Cách 1: Extends Thread class

```java
public class MyThread extends Thread {
    private String threadName;
    
    public MyThread(String name) {
        this.threadName = name;
    }
    
    @Override
    public void run() {
        System.out.println(threadName + " bắt đầu");
        
        for (int i = 1; i <= 5; i++) {
            System.out.println(threadName + ": " + i);
            
            try {
                Thread.sleep(500); // Ngủ 500ms
            } catch (InterruptedException e) {
                System.out.println(threadName + " bị gián đoạn");
            }
        }
        
        System.out.println(threadName + " kết thúc");
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("Thread-1");
        MyThread thread2 = new MyThread("Thread-2");
        
        thread1.start(); // Khởi động thread
        thread2.start();
    }
}
```

### Cách 2: Implements Runnable interface (Khuyến nghị)

```java
public class MyRunnable implements Runnable {
    private String taskName;
    
    public MyRunnable(String name) {
        this.taskName = name;
    }
    
    @Override
    public void run() {
        System.out.println(taskName + " bắt đầu");
        
        for (int i = 1; i <= 5; i++) {
            System.out.println(taskName + ": " + i);
            
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        
        System.out.println(taskName + " hoàn thành");
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new MyRunnable("Task-1"));
        Thread thread2 = new Thread(new MyRunnable("Task-2"));
        
        thread1.start();
        thread2.start();
    }
}
```

### Cách 3: Lambda Expression (Java 8+)

```java
public class LambdaThreadExample {
    public static void main(String[] args) {
        // Runnable với lambda
        Thread thread1 = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Thread 1: " + i);
                try {
                    Thread.sleep(300);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        Thread thread2 = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Thread 2: " + i);
                try {
                    Thread.sleep(300);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        thread1.start();
        thread2.start();
    }
}
```

## Thread Lifecycle

```
NEW → RUNNABLE → RUNNING → TERMINATED
         ↓           ↓
      BLOCKED    WAITING/TIMED_WAITING
```

## Join() - Đợi thread kết thúc

```java
public class JoinExample {
    public static void main(String[] args) {
        Thread downloadThread = new Thread(() -> {
            System.out.println("Đang tải file...");
            try {
                Thread.sleep(3000); // Giả lập tải file
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Tải file hoàn tất!");
        });
        
        downloadThread.start();
        
        try {
            downloadThread.join(); // Đợi downloadThread kết thúc
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("Bắt đầu xử lý file đã tải");
    }
}
```

## Synchronization - Đồng bộ hóa

Khi nhiều thread truy cập cùng một tài nguyên, cần đồng bộ hóa để tránh xung đột.

### Vấn đề Race Condition

```java
public class Counter {
    private int count = 0;
    
    public void increment() {
        count++; // Không thread-safe!
    }
    
    public int getCount() {
        return count;
    }
}

// Vấn đề
public class RaceConditionExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        t1.start();
        t2.start();
        
        t1.join();
        t2.join();
        
        // Kết quả không chính xác (không phải 2000)
        System.out.println("Count: " + counter.getCount());
    }
}
```

### Giải pháp: synchronized

```java
public class SynchronizedCounter {
    private int count = 0;
    
    // Synchronized method
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// Hoặc synchronized block
public class Counter {
    private int count = 0;
    private final Object lock = new Object();
    
    public void increment() {
        synchronized(lock) {
            count++;
        }
    }
}
```

## Ví dụ thực tế: Producer-Consumer

```java
import java.util.LinkedList;
import java.util.Queue;

public class ProducerConsumer {
    private static final int MAX_SIZE = 5;
    private Queue<Integer> queue = new LinkedList<>();
    
    // Producer
    public synchronized void produce(int value) throws InterruptedException {
        while (queue.size() == MAX_SIZE) {
            wait(); // Đợi khi queue đầy
        }
        
        queue.add(value);
        System.out.println("Produced: " + value + " | Queue size: " + queue.size());
        
        notifyAll(); // Thông báo cho consumer
    }
    
    // Consumer
    public synchronized int consume() throws InterruptedException {
        while (queue.isEmpty()) {
            wait(); // Đợi khi queue rỗng
        }
        
        int value = queue.poll();
        System.out.println("Consumed: " + value + " | Queue size: " + queue.size());
        
        notifyAll(); // Thông báo cho producer
        return value;
    }
    
    public static void main(String[] args) {
        ProducerConsumer pc = new ProducerConsumer();
        
        // Producer thread
        Thread producer = new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                try {
                    pc.produce(i);
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        // Consumer thread
        Thread consumer = new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                try {
                    pc.consume();
                    Thread.sleep(300);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        producer.start();
        consumer.start();
    }
}
```

## Thread Pool với ExecutorService

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ThreadPoolExample {
    public static void main(String[] args) {
        // Tạo thread pool với 3 threads
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Submit 10 tasks
        for (int i = 1; i <= 10; i++) {
            int taskId = i;
            executor.submit(() -> {
                System.out.println("Task " + taskId + " bắt đầu - Thread: " + 
                                 Thread.currentThread().getName());
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Task " + taskId + " hoàn thành");
            });
        }
        
        // Shutdown executor
        executor.shutdown();
        
        try {
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
        
        System.out.println("Tất cả tasks đã hoàn thành");
    }
}
```

## Best Practices

1. **Ưu tiên Runnable hơn Thread**: Linh hoạt hơn, có thể extend class khác
2. **Sử dụng Thread Pool**: Tiết kiệm tài nguyên, quản lý tốt hơn
3. **Tránh synchronized quá mức**: Có thể gây bottleneck
4. **Xử lý InterruptedException đúng cách**: Không bỏ qua exception
5. **Sử dụng concurrent collections**: ConcurrentHashMap, CopyOnWriteArrayList...

## Kết luận

Multithreading giúp:
- Tăng hiệu suất ứng dụng
- Tận dụng tối đa CPU
- Cải thiện trải nghiệm người dùng
- Xử lý nhiều tác vụ đồng thời

Nhưng cần cẩn thận với:
- Race conditions
- Deadlocks
- Thread safety

Hãy thực hành để làm chủ multithreading! 🚀
