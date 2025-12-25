---
title: "Java Collections Framework - ArrayList, LinkedList, HashMap"
date: 2025-12-18
draft: false
tags: ["Java", "Collections", "Data Structures"]
categories: ["Java"]
---

# Java Collections Framework

Collections Framework là một trong những phần quan trọng nhất của Java, cung cấp các cấu trúc dữ liệu và thuật toán phổ biến.

## List Interface

### ArrayList - Dynamic Array

ArrayList sử dụng mảng động để lưu trữ dữ liệu.

```java
import java.util.ArrayList;
import java.util.Collections;

public class ArrayListExample {
    public static void main(String[] args) {
        // Khởi tạo ArrayList
        ArrayList<String> fruits = new ArrayList<>();
        
        // Thêm phần tử
        fruits.add("Táo");
        fruits.add("Chuối");
        fruits.add("Cam");
        fruits.add("Xoài");
        
        // Thêm tại vị trí cụ thể
        fruits.add(1, "Dâu");
        
        // Truy cập phần tử
        System.out.println("Phần tử đầu tiên: " + fruits.get(0));
        
        // Cập nhật
        fruits.set(0, "Táo xanh");
        
        // Xóa phần tử
        fruits.remove("Cam");
        fruits.remove(0); // Xóa theo index
        
        // Kích thước
        System.out.println("Số phần tử: " + fruits.size());
        
        // Duyệt qua ArrayList
        System.out.println("\nDanh sách trái cây:");
        for (String fruit : fruits) {
            System.out.println("- " + fruit);
        }
        
        // Sắp xếp
        Collections.sort(fruits);
        System.out.println("\nSau khi sắp xếp: " + fruits);
        
        // Kiểm tra tồn tại
        if (fruits.contains("Chuối")) {
            System.out.println("Có chuối trong danh sách");
        }
        
        // Xóa tất cả
        fruits.clear();
        System.out.println("ArrayList rỗng? " + fruits.isEmpty());
    }
}
```

**Ưu điểm:**
- Truy cập nhanh theo index: O(1)
- Duyệt qua các phần tử nhanh

**Nhược điểm:**
- Thêm/xóa ở giữa chậm: O(n)
- Tốn bộ nhớ khi mảng phải mở rộng

### LinkedList - Danh sách liên kết

```java
import java.util.LinkedList;

public class LinkedListExample {
    public static void main(String[] args) {
        LinkedList<String> names = new LinkedList<>();
        
        // Thêm phần tử
        names.add("An");
        names.add("Bình");
        names.add("Chi");
        
        // Thêm vào đầu/cuối
        names.addFirst("Anh");
        names.addLast("Dũng");
        
        // Lấy phần tử đầu/cuối
        System.out.println("Đầu: " + names.getFirst());
        System.out.println("Cuối: " + names.getLast());
        
        // Xóa đầu/cuối
        names.removeFirst();
        names.removeLast();
        
        System.out.println("Danh sách: " + names);
        
        // Sử dụng như Queue
        LinkedList<String> queue = new LinkedList<>();
        queue.offer("Task 1"); // Thêm vào cuối
        queue.offer("Task 2");
        queue.offer("Task 3");
        
        System.out.println("\nXử lý Queue:");
        while (!queue.isEmpty()) {
            System.out.println("Xử lý: " + queue.poll()); // Lấy và xóa từ đầu
        }
        
        // Sử dụng như Stack
        LinkedList<String> stack = new LinkedList<>();
        stack.push("Item 1"); // Thêm vào đầu
        stack.push("Item 2");
        stack.push("Item 3");
        
        System.out.println("\nXử lý Stack:");
        while (!stack.isEmpty()) {
            System.out.println("Pop: " + stack.pop()); // Lấy và xóa từ đầu
        }
    }
}
```

**Ưu điểm:**
- Thêm/xóa ở đầu/cuối nhanh: O(1)
- Không cần mở rộng bộ nhớ

**Nhược điểm:**
- Truy cập theo index chậm: O(n)
- Tốn bộ nhớ cho con trỏ

## Map Interface

### HashMap - Key-Value Storage

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        // Khởi tạo HashMap
        HashMap<String, Integer> scores = new HashMap<>();
        
        // Thêm cặp key-value
        scores.put("An", 85);
        scores.put("Bình", 92);
        scores.put("Chi", 78);
        scores.put("Dũng", 95);
        
        // Truy cập giá trị
        System.out.println("Điểm của An: " + scores.get("An"));
        
        // Cập nhật giá trị
        scores.put("An", 90); // Ghi đè
        
        // Thêm nếu chưa tồn tại
        scores.putIfAbsent("An", 100); // Không thay đổi vì đã có
        scores.putIfAbsent("Em", 88);  // Thêm mới
        
        // Kiểm tra tồn tại
        if (scores.containsKey("Bình")) {
            System.out.println("Bình có trong danh sách");
        }
        
        if (scores.containsValue(95)) {
            System.out.println("Có người đạt 95 điểm");
        }
        
        // Lấy giá trị mặc định nếu không tồn tại
        int score = scores.getOrDefault("Phương", 0);
        System.out.println("Điểm của Phương: " + score);
        
        // Duyệt qua HashMap
        System.out.println("\nBảng điểm:");
        for (Map.Entry<String, Integer> entry : scores.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // Chỉ duyệt keys
        System.out.println("\nDanh sách học sinh:");
        for (String name : scores.keySet()) {
            System.out.println("- " + name);
        }
        
        // Chỉ duyệt values
        System.out.println("\nDanh sách điểm:");
        for (Integer score2 : scores.values()) {
            System.out.println("- " + score2);
        }
        
        // Xóa
        scores.remove("Chi");
        
        // Kích thước
        System.out.println("\nSố học sinh: " + scores.size());
    }
}
```

### Ví dụ thực tế: Đếm từ trong văn bản

```java
import java.util.HashMap;
import java.util.Map;

public class WordCounter {
    public static void main(String[] args) {
        String text = "Java là ngôn ngữ lập trình hướng đối tượng. " +
                     "Java được sử dụng rộng rãi trong phát triển ứng dụng. " +
                     "Học Java rất quan trọng.";
        
        // Tách từ và chuyển về chữ thường
        String[] words = text.toLowerCase()
                            .replaceAll("[^a-záàảãạâấầẩẫậăắằẳẵặéèẻẽẹêếềểễệíìỉĩịóòỏõọôốồổỗộơớờởỡợúùủũụưứừửữựýỳỷỹỵđ ]", "")
                            .split("\\s+");
        
        // Đếm tần suất
        HashMap<String, Integer> wordCount = new HashMap<>();
        
        for (String word : words) {
            if (!word.isEmpty()) {
                wordCount.put(word, wordCount.getOrDefault(word, 0) + 1);
            }
        }
        
        // Hiển thị kết quả
        System.out.println("Tần suất từ:");
        for (Map.Entry<String, Integer> entry : wordCount.entrySet()) {
            if (entry.getValue() > 1) {
                System.out.println(entry.getKey() + ": " + entry.getValue() + " lần");
            }
        }
    }
}
```

## So sánh ArrayList vs LinkedList

| Thao tác | ArrayList | LinkedList |
|----------|-----------|------------|
| get(index) | O(1) | O(n) |
| add(element) | O(1)* | O(1) |
| add(index, element) | O(n) | O(n) |
| remove(index) | O(n) | O(n) |
| addFirst/Last | O(n) | O(1) |
| removeFirst/Last | O(n) | O(1) |

*Trung bình O(1), worst case O(n) khi cần mở rộng mảng

## Khi nào dùng gì?

**ArrayList:**
- Khi cần truy cập ngẫu nhiên nhiều
- Ít thêm/xóa phần tử
- Biết trước số lượng phần tử

**LinkedList:**
- Khi thêm/xóa ở đầu/cuối nhiều
- Sử dụng như Queue hoặc Stack
- Không cần truy cập ngẫu nhiên

**HashMap:**
- Lưu trữ cặp key-value
- Cần tra cứu nhanh theo key
- Key là unique

## Kết luận

Collections Framework giúp:
- Tiết kiệm thời gian phát triển
- Code hiệu quả hơn
- Tránh phát minh lại bánh xe

Hãy chọn đúng Collection cho từng bài toán! 🎯
