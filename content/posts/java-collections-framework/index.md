---
title: "Java Collections Framework - ArrayList, LinkedList, HashMap"
date: 2025-12-18
draft: false
tags: ["Java", "Collections", "Data Structures"]
categories: ["Java"]
image: "images/posts/java-collections-framework.jpg"
description: "Bài viết hệ thống hoá Java Collections Framework với ví dụ trực quan cho ArrayList, LinkedList, HashSet, HashMap và gợi ý cách chọn collection theo nhu cầu thực tế."
---

Collections Framework (JCF) là bộ **interface + class** giúp bạn lưu trữ và thao tác dữ liệu theo nhiều cách: danh sách (List), tập hợp (Set), hàng đợi (Queue/Deque) và ánh xạ key-value (Map).

![Tóm tắt nhanh Java Collections Framework](collections-overview.svg)

## Mục tiêu của JCF

- **Chuẩn hoá API**: thay vì mỗi cấu trúc dữ liệu một kiểu, JCF gom về các interface chung.
- **Đổi implementation dễ**: bạn có thể thay `ArrayList` → `LinkedList` mà ít phải sửa code.
- **Hiệu năng rõ ràng**: mỗi cấu trúc dữ liệu có trade-off khác nhau.

## Chọn Collection như thế nào?

1) Hỏi: dữ liệu có **trùng lặp** không?
- Không trùng → `Set`
- Có trùng → `List`

2) Hỏi: có cần **thứ tự** không?
- Cần thứ tự theo insertion → `List` hoặc `LinkedHashSet`
- Cần sort tự nhiên/Comparator → `TreeSet` / `TreeMap`

3) Hỏi: có cần **key → value** không?
- Có → `Map`

## List Interface

List có **thứ tự** và **cho phép trùng**.

### ArrayList (default choice)

`ArrayList` phù hợp nhất cho đa số trường hợp: truy cập theo index nhanh, duyệt nhanh.

![ArrayList vs LinkedList](arraylist-vs-linkedlist.svg)

Ví dụ CRUD cơ bản:

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class ArrayListExample {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>();

        fruits.add("Táo");
        fruits.add("Chuối");
        fruits.add("Cam");
        fruits.add("Xoài");

        fruits.add(1, "Dâu");

        System.out.println("First: " + fruits.get(0));

        fruits.set(0, "Táo xanh");

        fruits.remove("Cam");
        fruits.remove(0);

        System.out.println("Size: " + fruits.size());

        Collections.sort(fruits);
        System.out.println("Sorted: " + fruits);

        System.out.println("Contains Chuối? " + fruits.contains("Chuối"));
    }
}
```

Khi nên dùng:
- `get(i)`/`set(i)` nhiều
- Duyệt list thường xuyên
- Append ở cuối list là chủ yếu

### LinkedList (dùng như Deque/Queue)

`LinkedList` thường hợp hơn nếu bạn thao tác **đầu/cuối** nhiều (Deque). Nếu bạn cần Queue/Deque hiệu năng tốt, ưu tiên `ArrayDeque`.

```java
import java.util.LinkedList;

public class LinkedListAsDeque {
    public static void main(String[] args) {
        LinkedList<String> deque = new LinkedList<>();

        deque.addLast("A");
        deque.addLast("B");
        deque.addFirst("HEAD");

        System.out.println(deque); // [HEAD, A, B]

        System.out.println("removeFirst: " + deque.removeFirst());
        System.out.println("removeLast: " + deque.removeLast());

        System.out.println(deque);
    }
}
```

## Set Interface

Set **không cho phép trùng lặp**. Hai khái niệm quan trọng:
- **Định nghĩa trùng** dựa vào `equals()` và `hashCode()` (với HashSet).
- Với TreeSet, trùng dựa vào `compareTo()`/Comparator.

### HashSet / LinkedHashSet / TreeSet

- `HashSet`: nhanh, không đảm bảo thứ tự.
- `LinkedHashSet`: giữ thứ tự insertion.
- `TreeSet`: luôn được sort.

```java
import java.util.*;

public class SetDemo {
    public static void main(String[] args) {
        Set<String> a = new HashSet<>(List.of("java", "java", "js"));
        Set<String> b = new LinkedHashSet<>(List.of("b", "a", "b", "c"));
        Set<String> c = new TreeSet<>(List.of("b", "a", "b", "c"));

        System.out.println(a); // [java, js] (thứ tự không đảm bảo)
        System.out.println(b); // [b, a, c]  (giữ insertion)
        System.out.println(c); // [a, b, c]  (sorted)
    }
}
```

## Map Interface

Map lưu theo **key → value**.

### HashMap (phổ biến nhất)

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();

        scores.put("An", 85);
        scores.put("Bình", 92);
        scores.put("Chi", 78);

        scores.put("An", 90); // ghi đè

        System.out.println(scores.get("An"));
        System.out.println(scores.getOrDefault("Không có", 0));

        for (Map.Entry<String, Integer> e : scores.entrySet()) {
            System.out.println(e.getKey() + ": " + e.getValue());
        }
    }
}
```

### Ví dụ thực tế: đếm tần suất từ

```java
import java.util.HashMap;
import java.util.Locale;
import java.util.Map;

public class WordCounter {
    public static void main(String[] args) {
        String text = "Java là ngôn ngữ lập trình hướng đối tượng. Java được dùng rất nhiều.";

        String[] words = normalize(text).split("\\s+");

        Map<String, Integer> freq = new HashMap<>();
        for (String w : words) {
            if (w.isBlank()) continue;
            freq.put(w, freq.getOrDefault(w, 0) + 1);
        }

        System.out.println(freq);
    }

    static String normalize(String input) {
        return input
                .toLowerCase(Locale.ROOT)
                .replaceAll("[^a-z0-9áàảãạâấầẩẫậăắằẳẵặéèẻẽẹêếềểễệíìỉĩịóòỏõọôốồổỗộơớờởỡợúùủũụưứừửữựýỳỷỹỵđ ]", " ")
                .trim();
    }
}
```

## Bảng so sánh nhanh

| Thao tác | ArrayList | LinkedList |
|----------|-----------|------------|
| get(index) | O(1) | O(n) |
| addLast | O(1)* | O(1) |
| add(index) | O(n) | O(n) |
| remove(index) | O(n) | O(n) |
| addFirst/removeFirst | O(n) | O(1) |

*Amortized O(1), đôi khi O(n) khi phải resize.

## Kết luận

- **Mặc định**: `ArrayList`, `HashMap`, `HashSet`.
- **Cần giữ thứ tự insertion**: `LinkedHashMap`, `LinkedHashSet`.
- **Cần sort**: `TreeMap`, `TreeSet`.
- **Cần Queue/Deque tốt**: `ArrayDeque`.
