---
title: "Lập trình hướng đối tượng (OOP) trong Java - Khái niệm cơ bản"
date: 2025-12-20
draft: false
tags: ["Java", "OOP", "Lập trình cơ bản"]
categories: ["Java"]
description: "Giới thiệu toàn diện về OOP trong Java với class, object, kế thừa, interface, abstract class, composition cùng các nguyên tắc giúp thiết kế code dễ bảo trì."
image: "images/posts/java-oop-basics.jpg"
---

# Lập trình hướng đối tượng trong Java

Lập trình hướng đối tượng (Object-Oriented Programming - OOP) là một paradigm lập trình quan trọng trong Java. Hãy cùng tìm hiểu các khái niệm cơ bản!

## Lý thuyết nền tảng về OOP trong Java

### Object và Class

- **Class** là “bản thiết kế” (blueprint): định nghĩa dữ liệu (field) và hành vi (method).
- **Object** là instance cụ thể được tạo từ class (`new`).

Trong thực tế, OOP giúp bạn mô hình hoá bài toán theo *đối tượng* thay vì chỉ theo hàm và dữ liệu rời rạc.

### Interface, abstract class và class thường

- **Interface**: mô tả “hợp đồng” (contract) – những method mà một class phải có. Hay dùng để decouple (phụ thuộc vào abstraction).
- **Abstract class**: vừa có thể có method abstract, vừa có thể chứa logic dùng chung/field. Dùng khi các class con có “phần chung” rõ ràng.
- **Concrete class**: triển khai đầy đủ.

Không có lựa chọn đúng tuyệt đối: interface tốt cho “vai trò”, abstract class tốt cho “có code dùng chung”.

### Composition vs Inheritance (ưu tiên composition khi có thể)

Inheritance giúp tái sử dụng, nhưng cũng dễ làm class bị “cứng” và khó mở rộng (tight coupling). Trong nhiều trường hợp, **composition** (has-a) linh hoạt hơn **inheritance** (is-a).

Ví dụ tư duy:

- `Car has-a Engine` (composition)
- `Student is-a Person` (inheritance)

### Một vài nguyên tắc giúp OOP “đúng nghĩa”

- **SRP** (Single Responsibility): mỗi class có 1 lý do để thay đổi.
- **Encapsulation**: che giấu state, public API nhỏ gọn.
- Tránh lộ “setter everywhere” nếu không cần; cân nhắc immutability khi phù hợp.

## 4 Trụ cột của OOP

### 1. Encapsulation (Đóng gói)

Đóng gói là việc che giấu dữ liệu bên trong class và chỉ cho phép truy cập thông qua các phương thức public.

```java
public class Student {
    // Private fields
    private String name;
    private int age;
    private double gpa;
    
    // Constructor
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
    
    // Getters và Setters
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        if (age > 0 && age < 100) {
            this.age = age;
        }
    }
    
    public double getGpa() {
        return gpa;
    }
    
    public void setGpa(double gpa) {
        if (gpa >= 0 && gpa <= 4.0) {
            this.gpa = gpa;
        }
    }
}
```

**Lợi ích:**
- Bảo vệ dữ liệu khỏi truy cập trực tiếp
- Kiểm soát việc đọc/ghi dữ liệu
- Dễ bảo trì và mở rộng

### 2. Inheritance (Kế thừa)

Kế thừa cho phép một class con thừa hưởng các thuộc tính và phương thức từ class cha.

```java
// Class cha
public class Person {
    protected String name;
    protected int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void introduce() {
        System.out.println("Tôi là " + name + ", " + age + " tuổi");
    }
}

// Class con
public class Student extends Person {
    private String studentId;
    private double gpa;
    
    public Student(String name, int age, String studentId, double gpa) {
        super(name, age); // Gọi constructor của class cha
        this.studentId = studentId;
        this.gpa = gpa;
    }
    
    @Override
    public void introduce() {
        super.introduce();
        System.out.println("MSSV: " + studentId + ", GPA: " + gpa);
    }
}
```

### 3. Polymorphism (Đa hình)

Đa hình cho phép một đối tượng có thể có nhiều hình thái khác nhau.

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow! Meow!");
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        Animal myAnimal;
        
        myAnimal = new Dog();
        myAnimal.makeSound(); // Dog barks: Woof! Woof!
        
        myAnimal = new Cat();
        myAnimal.makeSound(); // Cat meows: Meow! Meow!
    }
}
```

### 4. Abstraction (Trừu tượng)

Trừu tượng hóa là việc ẩn đi các chi tiết phức tạp và chỉ hiển thị các tính năng cần thiết.

```java
// Abstract class
public abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    // Abstract method
    public abstract double getArea();
    public abstract double getPerimeter();
    
    // Concrete method
    public void displayColor() {
        System.out.println("Màu sắc: " + color);
    }
}

public class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }
}
```

## Kết luận

OOP giúp code của bạn:
- Dễ đọc và dễ hiểu hơn
- Tái sử dụng code tốt hơn
- Dễ bảo trì và mở rộng
- Mô phỏng thế giới thực tốt hơn

Hãy thực hành nhiều để nắm vững các khái niệm OOP! 🚀
