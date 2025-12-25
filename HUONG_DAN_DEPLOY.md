# Hướng dẫn triển khai Blog lên GitHub Pages

## Bước 1: Cài đặt HUGO (nếu chưa có)

### Windows
Mở PowerShell với quyền Administrator và chạy:

```powershell
# Sử dụng Chocolatey
choco install hugo-extended -y

# Hoặc Scoop
scoop install hugo-extended

# Hoặc tải trực tiếp từ GitHub
# https://github.com/gohugoio/hugo/releases
```

Kiểm tra cài đặt:
```powershell
hugo version
```

## Bước 2: Tạo GitHub Repository

1. Đăng nhập vào GitHub
2. Tạo repository mới
3. Tên repository:
   - `<username>.github.io` (blog sẽ ở https://username.github.io)
   - Hoặc bất kỳ tên nào (blog sẽ ở https://username.github.io/repo-name)
4. Để Public
5. Không tạo README, .gitignore (đã có sẵn)

## Bước 3: Cập nhật cấu hình

Sửa file `hugo.toml`:

```toml
baseURL = "https://<username>.github.io/"  # Hoặc https://<username>.github.io/<repo-name>/
title = "Tên Blog của bạn"

[params]
  author = "Tên của bạn"
  description = "Mô tả blog của bạn"
```

Sửa file `content/about.md` với thông tin cá nhân.

## Bước 4: Push lên GitHub

Mở Terminal/PowerShell tại thư mục `D:\LTM` và chạy:

```bash
# Khởi tạo Git repository
git init

# Thêm tất cả files
git add .

# Commit
git commit -m "Initial commit: HUGO blog with Java & JavaScript posts"

# Đổi branch thành main
git branch -M main

# Thêm remote (thay <username> và <repo-name>)
git remote add origin https://github.com/<username>/<repo-name>.git

# Push lên GitHub
git push -u origin main
```

## Bước 5: Cấu hình GitHub Pages

1. Vào repository trên GitHub
2. Click **Settings**
3. Click **Pages** (menu bên trái)
4. Trong **Source**, chọn **GitHub Actions**
5. Xong!

## Bước 6: Đợi Deploy

1. Vào tab **Actions** trên GitHub
2. Xem workflow "Deploy Hugo site to GitHub Pages" đang chạy
3. Đợi khoảng 1-2 phút
4. Khi có dấu ✓ xanh, blog đã được deploy

## Bước 7: Truy cập Blog

Mở browser và vào:
- `https://<username>.github.io/` (nếu dùng username.github.io repo)
- `https://<username>.github.io/<repo-name>/` (nếu dùng repo khác)

## Test local trước khi deploy

```bash
# Chạy development server
hugo server -D

# Mở browser tại http://localhost:1313
```

## Cập nhật blog sau này

```bash
# Tạo bài viết mới
hugo new posts/ten-bai-viet-moi.md

# Hoặc tạo file thủ công trong content/posts/

# Sau khi viết xong, push lên GitHub
git add .
git commit -m "Add new post: Tên bài viết"
git push

# GitHub Actions sẽ tự động deploy!
```

## Troubleshooting

### Lỗi: baseURL không đúng

Nếu CSS/link không load, kiểm tra `baseURL` trong `hugo.toml` phải khớp với URL thực tế.

### Lỗi: GitHub Actions failed

1. Vào tab **Actions**
2. Click vào workflow failed
3. Xem logs để tìm lỗi
4. Thường do:
   - baseURL không đúng
   - Hugo version không tương thích
   - Syntax error trong content

### Lỗi: Page 404

1. Kiểm tra GitHub Pages settings
2. Đảm bảo Source = "GitHub Actions"
3. Đợi vài phút sau khi deploy

## Tính năng đã có

✅ 9 bài viết (5 Java + 4 JavaScript) bằng tiếng Việt
✅ Trang Home với danh sách bài viết
✅ Trang Blog (Posts)
✅ Trang Giới thiệu (About)
✅ Theme tối giản, responsive
✅ Syntax highlighting cho code
✅ Tags và Categories
✅ Auto deploy với GitHub Actions
✅ Menu navigation
✅ SEO friendly

## Nội dung Blog

### Java (5 bài)
1. Lập trình hướng đối tượng (OOP)
2. Xử lý ngoại lệ (Exception Handling)
3. Collections Framework (ArrayList, HashMap)
4. Multithreading (Lập trình đa luồng)
5. Socket Programming (TCP/IP)

### JavaScript (4 bài)
1. JavaScript cơ bản (Biến, kiểu dữ liệu, toán tử)
2. Functions (Arrow functions, Higher-order functions)
3. Async JavaScript (Promises, Async/Await)
4. DOM Manipulation

## Liên hệ và hỗ trợ

Nếu cần thêm tính năng hoặc có vấn đề, hãy:
1. Kiểm tra file README.md
2. Xem HUGO documentation: https://gohugo.io/documentation/
3. GitHub Pages docs: https://docs.github.com/en/pages

---

**Chúc bạn thành công! 🎉**
