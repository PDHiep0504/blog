---
title: "Xử lý ngoại lệ (Exception Handling) trong Java"
date: 2025-12-19
draft: false
tags: ["Java", "Exception", "Error Handling"]
categories: ["Java"]
---

# Xử lý ngoại lệ trong Java

Xử lý ngoại lệ (Exception Handling) là một phần quan trọng trong lập trình Java, giúp chương trình xử lý các lỗi một cách an toàn.

## Phân loại Exception

### 1. Checked Exception
Được kiểm tra tại compile-time, phải được xử lý bằng try-catch hoặc throws.

```java
import java.io.*;

public class FileExample {
    // Phải khai báo throws hoặc dùng try-catch
    public static void readFile(String fileName) throws IOException {
        FileReader file = new FileReader(fileName);
        BufferedReader reader = new BufferedReader(file);
        
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
        
        reader.close();
    }
}
```

### 2. Unchecked Exception (Runtime Exception)
Xảy ra trong quá trình runtime, không bắt buộc phải xử lý.

```java
public class RuntimeExceptionExample {
    public static void main(String[] args) {
        // NullPointerException
        String str = null;
        // System.out.println(str.length()); // Lỗi!
        
        // ArithmeticException
        // int result = 10 / 0; // Lỗi!
        
        // ArrayIndexOutOfBoundsException
        int[] numbers = {1, 2, 3};
        // System.out.println(numbers[5]); // Lỗi!
    }
}
```

## Cấu trúc Try-Catch-Finally

### Cơ bản

```java
public class TryCatchExample {
    public static void main(String[] args) {
        try {
            int[] numbers = {1, 2, 3};
            System.out.println(numbers[5]); // Lỗi!
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Lỗi: Vượt quá chỉ số mảng!");
            System.out.println("Chi tiết: " + e.getMessage());
        } finally {
            System.out.println("Block finally luôn được thực thi");
        }
    }
}
```

### Nhiều Catch Block

```java
public class MultipleCatchExample {
    public static void divide(String num1, String num2) {
        try {
            int a = Integer.parseInt(num1);
            int b = Integer.parseInt(num2);
            int result = a / b;
            System.out.println("Kết quả: " + result);
        } catch (NumberFormatException e) {
            System.out.println("Lỗi: Định dạng số không hợp lệ!");
        } catch (ArithmeticException e) {
            System.out.println("Lỗi: Không thể chia cho 0!");
        } catch (Exception e) {
            System.out.println("Lỗi không xác định: " + e.getMessage());
        }
    }
    
    public static void main(String[] args) {
        divide("10", "2");    // OK
        divide("10", "0");    // ArithmeticException
        divide("abc", "5");   // NumberFormatException
    }
}
```

## Try-with-Resources (Java 7+)

Tự động đóng tài nguyên sau khi sử dụng.

```java
import java.io.*;

public class TryWithResourcesExample {
    public static void readFile(String fileName) {
        // Tự động đóng file sau khi sử dụng
        try (BufferedReader reader = new BufferedReader(new FileReader(fileName))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Lỗi đọc file: " + e.getMessage());
        }
        // Không cần gọi reader.close() - tự động đóng!
    }
}
```

## Tạo Custom Exception

```java
// Custom Exception class
public class InvalidAgeException extends Exception {
    private int age;
    
    public InvalidAgeException(int age) {
        super("Tuổi không hợp lệ: " + age);
        this.age = age;
    }
    
    public int getAge() {
        return age;
    }
}

// Sử dụng Custom Exception
public class Person {
    private String name;
    private int age;
    
    public void setAge(int age) throws InvalidAgeException {
        if (age < 0 || age > 150) {
            throw new InvalidAgeException(age);
        }
        this.age = age;
    }
}

// Test
public class Main {
    public static void main(String[] args) {
        Person person = new Person();
        
        try {
            person.setAge(-5);
        } catch (InvalidAgeException e) {
            System.out.println("Lỗi: " + e.getMessage());
            System.out.println("Tuổi nhập vào: " + e.getAge());
        }
    }
}
```

## Best Practices

### 1. Xử lý Exception cụ thể

```java
// ❌ Không nên
try {
    // code
} catch (Exception e) {
    e.printStackTrace();
}

// ✅ Nên
try {
    // code
} catch (FileNotFoundException e) {
    System.out.println("File không tồn tại");
} catch (IOException e) {
    System.out.println("Lỗi đọc/ghi file");
}
```

### 2. Không bỏ qua Exception

```java
// ❌ Không nên
try {
    // code
} catch (Exception e) {
    // Bỏ qua - rất nguy hiểm!
}

// ✅ Nên
try {
    // code
} catch (Exception e) {
    logger.error("Lỗi xảy ra", e);
    // hoặc xử lý phù hợp
}
```

### 3. Sử dụng Finally cho cleanup

```java
Connection conn = null;
try {
    conn = getConnection();
    // Xử lý database
} catch (SQLException e) {
    System.out.println("Lỗi database: " + e.getMessage());
} finally {
    if (conn != null) {
        try {
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## Kết luận

Exception handling giúp:
- Chương trình ổn định hơn
- Dễ debug và bảo trì
- Trải nghiệm người dùng tốt hơn
- Code rõ ràng và chuyên nghiệp

Hãy luôn xử lý exception một cách hợp lý! 💪
