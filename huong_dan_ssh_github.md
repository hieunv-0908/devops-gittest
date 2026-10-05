# Hướng Dẫn Cấu Hình SSH Key Cho GitHub

Tài liệu này ghi lại các bước chi tiết để thiết lập, kiểm tra và khắc phục lỗi kết nối SSH Key giữa máy tính cá nhân và GitHub.

---

## 1. Kiểm Tra Kết Nối Hiện Tại

Mở Terminal (hoặc Git Bash) và chạy lệnh:

```bash
ssh -T git@github.com
```

- **Thành công:** Màn hình hiển thị: `Hi <username>! You've successfully authenticated...`
- **Thất bại:** Báo lỗi `git@github.com: Permission denied (publickey)`.

---

## 2. Các Bước Khắc Phục Lỗi `Permission Denied`

### Bước 1: Khởi động SSH Agent và Thêm Khóa

1. Bật dịch vụ `ssh-agent`:
   ```bash
   eval "$(ssh-agent -s)"
   ```
2. Thêm khóa SSH (dùng loại `ed25519`):
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

### Bước 2: Lấy Nội Dung Public Key

Chạy lệnh sau để hiển thị nội dung khóa công khai:

* **Trên Git Bash / Linux / macOS:**
  ```bash
  cat ~/.ssh/id_ed25519.pub
  ```
* **Trên Windows PowerShell:**
  ```powershell
  Get-Content ~/.ssh/id_ed25519.pub
  ```

> **Lưu ý:** Sao chép toàn bộ chuỗi ký tự bắt đầu bằng `ssh-ed25519 ...`.

### Bước 3: Thêm Key Vào Tài Khoản GitHub

1. Truy cập **GitHub.com** và đăng nhập.
2. Vào **Settings** $\rightarrow$ **SSH and GPG keys**.
3. Nhấn **New SSH key**.
4. Nhập **Title** (ví dụ: *Laptop-Work*) và dán nội dung đã copy ở Bước 2 vào ô **Key**.
5. Nhấn **Add SSH key** để xác nhận.

---

## 3. Chuyển Đổi URL Remote Trong Dự Án

Nếu kho lưu trữ (repository) đang dùng giao thức HTTPS, hãy đổi sang SSH:

```bash
# Kiểm tra URL hiện tại
git remote -v

# Đổi sang SSH
git remote set-url origin git@github.com:<username>/<repo-name>.git
```