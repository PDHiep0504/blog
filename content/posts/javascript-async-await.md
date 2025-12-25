---
title: "Async JavaScript - Promises và Async/Await"
date: 2025-12-13
draft: false
tags: ["JavaScript", "Async", "Promises", "Async/Await"]
categories: ["JavaScript"]
---

# Lập trình bất đồng bộ trong JavaScript

JavaScript là single-threaded, nhưng có thể xử lý nhiều tác vụ bất đồng bộ nhờ Event Loop. Hãy cùng tìm hiểu!

## Callback Functions

Cách truyền thống để xử lý async code.

### Callback đơn giản

```javascript
// Giả lập API call
function fetchUser(userId, callback) {
    console.log("Đang tải user...");
    
    setTimeout(() => {
        const user = {
            id: userId,
            name: "Nguyễn Văn An",
            email: "an@example.com"
        };
        callback(user);
    }, 1000);
}

// Sử dụng
fetchUser(1, (user) => {
    console.log("User loaded:", user);
});
```

### Callback Hell

Vấn đề khi có nhiều async operations phụ thuộc lẫn nhau.

```javascript
// ❌ Callback Hell - Khó đọc và maintain
getUserData(userId, (user) => {
    getPosts(user.id, (posts) => {
        getComments(posts[0].id, (comments) => {
            getLikes(comments[0].id, (likes) => {
                console.log(likes);
                // Pyramid of Doom! 😱
            });
        });
    });
});
```

## Promises

Promise là object đại diện cho kết quả của async operation.

### Tạo Promise

```javascript
// Promise có 3 trạng thái: pending, fulfilled, rejected
function fetchUser(userId) {
    return new Promise((resolve, reject) => {
        console.log("Đang tải user...");
        
        setTimeout(() => {
            if (userId > 0) {
                const user = {
                    id: userId,
                    name: "Nguyễn Văn An",
                    email: "an@example.com"
                };
                resolve(user);  // Thành công
            } else {
                reject(new Error("Invalid user ID"));  // Thất bại
            }
        }, 1000);
    });
}
```

### Sử dụng Promise với then/catch

```javascript
fetchUser(1)
    .then(user => {
        console.log("Success:", user);
        return user.id;
    })
    .then(userId => {
        console.log("User ID:", userId);
    })
    .catch(error => {
        console.error("Error:", error.message);
    })
    .finally(() => {
        console.log("Đã xong!");
    });
```

### Promise Chaining

```javascript
function getUser(userId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({id: userId, name: "An"});
        }, 500);
    });
}

function getPosts(userId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve([
                {id: 1, title: "Post 1"},
                {id: 2, title: "Post 2"}
            ]);
        }, 500);
    });
}

function getComments(postId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve([
                {id: 1, text: "Comment 1"},
                {id: 2, text: "Comment 2"}
            ]);
        }, 500);
    });
}

// ✅ Promise chaining - Dễ đọc hơn callback hell
getUser(1)
    .then(user => {
        console.log("User:", user);
        return getPosts(user.id);
    })
    .then(posts => {
        console.log("Posts:", posts);
        return getComments(posts[0].id);
    })
    .then(comments => {
        console.log("Comments:", comments);
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

## Promise Methods

### Promise.all() - Chạy song song

Chờ tất cả promises hoàn thành (hoặc một promise bị reject).

```javascript
const promise1 = Promise.resolve(10);
const promise2 = Promise.resolve(20);
const promise3 = Promise.resolve(30);

Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log(results);  // [10, 20, 30]
        const sum = results.reduce((a, b) => a + b);
        console.log("Sum:", sum);  // 60
    });

// Ví dụ thực tế: Load nhiều users cùng lúc
function fetchMultipleUsers() {
    const userIds = [1, 2, 3, 4, 5];
    
    const promises = userIds.map(id => fetchUser(id));
    
    Promise.all(promises)
        .then(users => {
            console.log("All users loaded:", users);
        })
        .catch(error => {
            console.error("One or more failed:", error);
        });
}
```

### Promise.race() - Lấy kết quả đầu tiên

```javascript
const slow = new Promise(resolve => {
    setTimeout(() => resolve("Slow"), 2000);
});

const fast = new Promise(resolve => {
    setTimeout(() => resolve("Fast"), 500);
});

Promise.race([slow, fast])
    .then(result => {
        console.log(result);  // "Fast"
    });

// Ví dụ: Timeout
function fetchWithTimeout(url, timeout) {
    const fetchPromise = fetch(url);
    
    const timeoutPromise = new Promise((_, reject) => {
        setTimeout(() => reject(new Error("Timeout")), timeout);
    });
    
    return Promise.race([fetchPromise, timeoutPromise]);
}
```

### Promise.allSettled() - Chờ tất cả (ES2020)

Chờ tất cả promises hoàn thành (không quan tâm fulfilled hay rejected).

```javascript
const promises = [
    Promise.resolve("Success 1"),
    Promise.reject("Error 1"),
    Promise.resolve("Success 2")
];

Promise.allSettled(promises)
    .then(results => {
        results.forEach((result, index) => {
            if (result.status === "fulfilled") {
                console.log(`${index}: Success -`, result.value);
            } else {
                console.log(`${index}: Failed -`, result.reason);
            }
        });
    });
```

### Promise.any() - Lấy thành công đầu tiên (ES2021)

```javascript
const promise1 = Promise.reject("Error 1");
const promise2 = new Promise(resolve => setTimeout(() => resolve("Success 2"), 100));
const promise3 = new Promise(resolve => setTimeout(() => resolve("Success 3"), 200));

Promise.any([promise1, promise2, promise3])
    .then(result => {
        console.log(result);  // "Success 2"
    });
```

## Async/Await

Cách viết async code trông giống synchronous code (ES2017).

### Cú pháp cơ bản

```javascript
// Async function luôn return Promise
async function fetchUserData(userId) {
    try {
        const user = await fetchUser(userId);
        console.log("User:", user);
        
        const posts = await getPosts(user.id);
        console.log("Posts:", posts);
        
        const comments = await getComments(posts[0].id);
        console.log("Comments:", comments);
        
        return comments;
    } catch (error) {
        console.error("Error:", error);
        throw error;
    }
}

// Gọi async function
fetchUserData(1)
    .then(comments => console.log("Done:", comments))
    .catch(error => console.error("Failed:", error));
```

### Async/Await với Arrow Function

```javascript
const loadData = async () => {
    const data = await fetchData();
    return data;
};

// IIFE (Immediately Invoked Function Expression)
(async () => {
    const user = await fetchUser(1);
    console.log(user);
})();
```

### Error Handling

```javascript
async function getUserInfo(userId) {
    try {
        const user = await fetchUser(userId);
        const posts = await getPosts(user.id);
        return {user, posts};
    } catch (error) {
        console.error("Error:", error.message);
        return null;
    }
}

// Hoặc handle ở nơi gọi
getUserInfo(1)
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

### Parallel Execution với Async/Await

```javascript
// ❌ Sequential - Chậm (3 giây)
async function loadSequential() {
    const user1 = await fetchUser(1);     // 1s
    const user2 = await fetchUser(2);     // 1s
    const user3 = await fetchUser(3);     // 1s
    return [user1, user2, user3];
}

// ✅ Parallel - Nhanh (1 giây)
async function loadParallel() {
    const [user1, user2, user3] = await Promise.all([
        fetchUser(1),
        fetchUser(2),
        fetchUser(3)
    ]);
    return [user1, user2, user3];
}

// Hoặc
async function loadParallel2() {
    const promise1 = fetchUser(1);
    const promise2 = fetchUser(2);
    const promise3 = fetchUser(3);
    
    const user1 = await promise1;
    const user2 = await promise2;
    const user3 = await promise3;
    
    return [user1, user2, user3];
}
```

## Ví dụ thực tế: Fetch API

### Với Promises

```javascript
fetch('https://api.example.com/users/1')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(user => {
        console.log(user);
        return fetch(`https://api.example.com/users/${user.id}/posts`);
    })
    .then(response => response.json())
    .then(posts => {
        console.log(posts);
    })
    .catch(error => {
        console.error('Fetch error:', error);
    });
```

### Với Async/Await

```javascript
async function getUserPosts(userId) {
    try {
        // Fetch user
        const userResponse = await fetch(`https://api.example.com/users/${userId}`);
        
        if (!userResponse.ok) {
            throw new Error(`HTTP error! status: ${userResponse.status}`);
        }
        
        const user = await userResponse.json();
        console.log("User:", user);
        
        // Fetch posts
        const postsResponse = await fetch(`https://api.example.com/users/${user.id}/posts`);
        const posts = await postsResponse.json();
        console.log("Posts:", posts);
        
        return {user, posts};
        
    } catch (error) {
        console.error("Error:", error.message);
        throw error;
    }
}

// Sử dụng
getUserPosts(1)
    .then(data => console.log("Success:", data))
    .catch(error => console.error("Failed:", error));
```

## Loop với Async/Await

```javascript
// Sequential processing
async function processUsers(userIds) {
    const results = [];
    
    for (const id of userIds) {
        const user = await fetchUser(id);
        results.push(user);
    }
    
    return results;
}

// Parallel processing
async function processUsersParallel(userIds) {
    const promises = userIds.map(id => fetchUser(id));
    const results = await Promise.all(promises);
    return results;
}

// Sử dụng
(async () => {
    const userIds = [1, 2, 3, 4, 5];
    
    console.time("Sequential");
    await processUsers(userIds);
    console.timeEnd("Sequential");  // ~5s
    
    console.time("Parallel");
    await processUsersParallel(userIds);
    console.timeEnd("Parallel");    // ~1s
})();
```

## Best Practices

1. **Luôn handle errors**: Dùng try/catch hoặc .catch()
2. **Prefer async/await**: Dễ đọc hơn promise chains
3. **Parallel when possible**: Dùng Promise.all() cho independent tasks
4. **Don't forget await**: Dễ quên await dẫn đến bugs
5. **Return values properly**: Async functions return Promises

```javascript
// ❌ Quên await
async function bad() {
    const user = fetchUser(1);  // Promise object, not data!
    console.log(user.name);     // undefined
}

// ✅ Đúng
async function good() {
    const user = await fetchUser(1);
    console.log(user.name);     // "Nguyễn Văn An"
}
```

## Kết luận

Async JavaScript:
- **Callbacks**: Cũ, dễ callback hell
- **Promises**: Modern, chainable
- **Async/Await**: Dễ đọc nhất, prefer for new code
- **Promise methods**: all, race, allSettled, any

Master async programming để build responsive apps! 🚀
