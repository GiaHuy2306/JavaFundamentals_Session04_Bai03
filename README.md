# Báo cáo Bài 3: Cấu hình xác thực SSH và Đẩy dự án lên GitHub

- **Học viên:** Huy-Tuan
- **Email:** sieuka1990@gmail.com
- **Đường dẫn nộp bài:** `homework/session_04/ex3/README.md`
- **URL Repository GitHub:** https://github.com/GiaHuy2306/JavaFundamentals_Session04_Bai03

---

## 1. Mục tiêu bài thực hành
- Tạo cặp khóa SSH an toàn sử dụng thuật toán **Ed25519**.
- Đăng ký Public Key lên tài khoản GitHub để xác thực không cần mật khẩu.
- Kiểm tra kết nối an toàn tới GitHub qua lệnh `ssh -T git@github.com`.
- Cấu hình remote repository theo định dạng SSH (`git@github.com:...`).
- Thực hiện đẩy mã nguồn (push) thành công từ local lên GitHub bằng giao thức SSH.

---

## 2. Quá trình tạo khóa SSH (Thuật toán Ed25519)

### 2.1. Sinh cặp khóa SSH
Chạy lệnh trong PowerShell:
```powershell
ssh-keygen -t ed25519 -C "sieuka1990@gmail.com"
```

Quá trình sinh khóa đã tạo ra 2 tệp tại thư mục `C:\Users\Admin\.ssh\`:
- **`id_ed25519`**: Khóa bí mật (Private Key) - **Tuyệt đối không chia sẻ hoặc nộp file này**.
- **`id_ed25519.pub`**: Khóa công khai (Public Key) - Dùng để đăng ký với GitHub.

### 2.2. Lấy nội dung Public Key để thêm vào GitHub
Chạy lệnh:
```powershell
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub"
```
Nội dung Public Key:
```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIfEkH5VV527aUtefGKQ22WB8+e1Wj6+yRETNBky/aio sieuka1990@gmail.com
```

### 2.3. Các bước thêm khóa vào GitHub:
1. Đăng nhập vào [GitHub](https://github.com).
2. Vào **Settings** $\rightarrow$ **SSH and GPG keys**.
3. Nhấn nút **New SSH key**.
4. Điền **Title** (ví dụ: `Windows-Laptop-Ed25519`), mục **Key type** để `Authentication Key`.
5. Dán toàn bộ chuỗi Public Key ở trên vào ô **Key** và nhấn **Add SSH key**.

---

## 3. Kiểm tra kết nối SSH tới GitHub

Sau khi đã thêm Public Key lên GitHub, chạy lệnh kiểm tra:
```bash
ssh -T git@github.com
```

### Kết quả mong đợi:
```text
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```
> Thông báo trên xác nhận danh tính tài khoản GitHub đã được chứng thực thành công qua khóa SSH Ed25519.

---

## 4. Cấu hình liên kết Remote Repository và Push mã nguồn

### 4.1. Tạo mới Repository trên GitHub
1. Truy cập [GitHub Create a new repository](https://github.com/new).
2. Đặt tên repository (ví dụ: `session04-ex3` hoặc tên repo bài tập của bạn).
3. Chọn chế độ **Public** (hoặc Private).
4. **Không** chọn *Add a README file*. Nhấn **Create repository**.

### 4.2. Thêm Remote URL định dạng SSH
Tại thư mục dự án cục bộ, chạy lệnh:
```bash
# Đổi username và repository theo tài khoản GitHub của bạn
git remote add origin git@github.com:<username>/<repository>.git
```

### 4.3. Kiểm tra cấu hình remote URL
Chạy lệnh:
```bash
git remote -v
```

**Kết quả mong đợi:**
```text
origin  git@github.com:<username>/<repository>.git (fetch)
origin  git@github.com:<username>/<repository>.git (push)
```
*(Đảm bảo URL bắt đầu bằng `git@github.com:` thay vì `https://`)*

### 4.4. Đẩy (push) mã nguồn lên GitHub
```bash
git branch -M main
git push -u origin main
```
Quá trình push diễn ra tự động thông qua SSH key mà không yêu cầu nhập mật khẩu hay Personal Access Token (PAT).
