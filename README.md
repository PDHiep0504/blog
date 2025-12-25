# Hugo Blog - Lập trình mạng

Blog cá nhân chia sẻ kiến thức về lập trình Java và JavaScript.

## Giới thiệu

Blog này được xây dựng bằng [HUGO](https://gohugo.io/) - Static Site Generator nhanh và mạnh mẽ.

### Nội dung

- **Java**: OOP, Exception Handling, Collections, Multithreading, Socket Programming
- **JavaScript**: Basics, Functions, Async/Await, DOM Manipulation
- **Network Programming**: TCP/IP, Socket, HTTP/HTTPS

## Cài đặt và chạy local

### Yêu cầu

- HUGO Extended (phiên bản >= 0.110.0)
- Git

### Cài đặt HUGO

**Windows (Chocolatey):**
```bash
choco install hugo-extended
```

**Windows (Winget):**
```bash
winget install Hugo.Hugo.Extended
```

**macOS:**
```bash
brew install hugo
```

**Linux:**
```bash
sudo apt-get install hugo
```

### Chạy blog

```bash
# Clone repository
git clone https://github.com/yourusername/ltm-blog.git
cd ltm-blog

# Chạy development server
hugo server -D

# Mở browser tại http://localhost:1313
```

## Build

```bash
# Build static files
hugo

# Output sẽ ở thư mục /public
```

## Deploy lên GitHub Pages

### Bước 1: Tạo GitHub Repository

1. Tạo repository mới trên GitHub
2. Repository name: `yourusername.github.io` (hoặc bất kỳ tên nào)

### Bước 2: Cấu hình GitHub Pages

1. Vào **Settings** > **Pages**
2. Source: **GitHub Actions**

### Bước 3: Push code

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/yourusername/your-repo.git
git push -u origin main
```

### Bước 4: Cập nhật baseURL

Sửa file `hugo.toml`:
```toml
baseURL = "https://yourusername.github.io/"
```

Hoặc nếu dùng custom repository:
```toml
baseURL = "https://yourusername.github.io/your-repo/"
```

### Bước 5: Deploy tự động

Mỗi lần push lên branch `main`, GitHub Actions sẽ tự động build và deploy.

Kiểm tra tiến trình tại: **Actions** tab trên GitHub.

## Cấu trúc thư mục

```
ltm-blog/
├── .github/
│   └── workflows/
│       └── hugo.yml          # GitHub Actions workflow
├── content/
│   ├── _index.md             # Trang chủ
│   ├── about.md              # Trang giới thiệu
│   └── posts/                # Các bài viết
│       ├── java-oop-basics.md
│       ├── java-exception-handling.md
│       ├── java-collections-framework.md
│       ├── java-multithreading.md
│       ├── java-socket-programming.md
│       ├── javascript-basics.md
│       ├── javascript-functions.md
│       ├── javascript-async-await.md
│       └── javascript-dom-manipulation.md
├── themes/
│   └── minimalist/           # Theme tối giản
│       ├── layouts/
│       └── theme.toml
├── hugo.toml                 # File cấu hình chính
└── README.md
```

## Viết bài mới

Tạo bài viết mới:

```bash
hugo new posts/ten-bai-viet.md
```

Hoặc tạo file thủ công trong `content/posts/` với front matter:

```markdown
---
title: "Tiêu đề bài viết"
date: 2025-12-25
draft: false
tags: ["Java", "Programming"]
categories: ["Java"]
---

Nội dung bài viết...
```

## Tính năng

- ✅ Responsive design
- ✅ Minimalist & clean UI
- ✅ Syntax highlighting cho code
- ✅ Tags và categories
- ✅ Tự động deploy với GitHub Actions
- ✅ Fast loading với HUGO
- ✅ SEO friendly

## Tùy chỉnh

### Thay đổi thông tin cá nhân

Sửa file `hugo.toml`:

```toml
title = "Tên Blog của bạn"

[params]
  author = "Tên của bạn"
  description = "Mô tả blog"
  subtitle = "Slogan của blog"
```

Sửa file `content/about.md` để cập nhật thông tin cá nhân.

### Thêm menu items

Sửa trong `hugo.toml`:

```toml
[[menu.main]]
  identifier = "contact"
  name = "Liên hệ"
  url = "/contact/"
  weight = 4
```

## License

MIT License - Free to use and modify.

## Liên hệ

- **Email**: your.email@example.com
- **GitHub**: [yourusername](https://github.com/yourusername)

---

**Happy Coding! 🚀**
