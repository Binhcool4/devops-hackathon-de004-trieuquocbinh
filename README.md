# DevOps Hackathon De004 – Triệu Quốc Bình

## Thông tin sinh viên

| Trường          | Giá trị                                |
|-----------------|----------------------------------------|
| Họ tên          | Triệu Quốc Bình                        |
| Linux username  | `trieuquocbinh-ks24cntt2`              |
| Lớp             | K24 CNTT2                              |
| Trường          | PTIT                                   |
| Repository      | `devops-hackathon-de004-trieuquocbinh` |
| Nhánh           | `main`                                 |
| PORT cá nhân    | **8083**                               |

---

## Cấu trúc repository

```
devops-hackathon-de002-phamvietquan/
├── src/
│   └── index.html                  ← Trang web thông tin cá nhân
├── nginx/
│   └── trieuquocbinh-ks24cntt2.conf  ← Server block Nginx
├── .gitignore
└── README.md
```

---

## Thông số triển khai

| Thông số            | Giá trị                                            |
|---------------------|----------------------------------------------------|
| Hệ điều hành        | Ubuntu 24.04                                       |
| Thư mục triển khai  | `/var/www/devops-hackathon-de004/trieuquocbinh`     |
| Web root (Nginx)    | `/var/www/devops-hackathon-de002/trieuquocbinh/src` |
| Cổng Nginx (PORT)   | `8083`                                             |
| `server_name`       | Địa chỉ IP máy chủ (xem `hostname -I`)             |
| File server block   | `trieuquocbinh-ks24cntt2.conf`                       |
| UFW rule            | `sudo ufw allow 8083/tcp`                          |
| URL truy cập        | `http://<IP_máy_chủ>:8083`                         |

---

## Hướng dẫn triển khai

### Phần 1 – Chuẩn bị môi trường

#### 1.1 Tạo tài khoản người dùng

```bash
# Tạo user với home dir và shell /bin/bash
sudo useradd -m -s /bin/bash trieuquocbinh-ks24cntt2

# Đặt mật khẩu
sudo passwd trieuquocbinh-ks24cntt2

# Thêm vào group sudo (secondary group)
sudo usermod -aG sudo trieuquocbinh-ks24cntt2

# Kiểm tra
id trieuquocbinh-ks24cntt2
```

#### 1.2 Cài đặt phần mềm

```bash
sudo apt update && sudo apt install -y nginx git ufw curl

# Bật và khởi động Nginx
sudo systemctl enable nginx
sudo systemctl start nginx

# Cấu hình Git
git config --global user.name "trieuquocbinh-ks24cntt2"
git config --global user.email "your-github-email@example.com"
```

---

### Phần 2 – Quản lý mã nguồn với Git/GitHub

```bash
# Clone repository về máy chủ
cd /var/www/devops-hackathon-de004/trieuquocbinh
git clone https://github.com/<github-username>/devops-hackathon-de004-trieuquocbinh.git .

# Hoặc pull khi đã clone rồi
git pull origin main
```

---

### Phần 3 – Cấu hình Nginx

```bash
# Copy server block vào sites-available
sudo cp nginx/trieuquocbinh-k24cntt2.conf /etc/nginx/sites-available/trieuquocbinh-k24cntt2.conf

# Tạo symlink sang sites-enabled
sudo ln -s /etc/nginx/sites-available/trieuquocbinh-k24cntt2.conf \
           /etc/nginx/sites-enabled/trieuquocbinh-k24cntt2.conf

# Xoá site default để tránh xung đột cổng 80
sudo rm -f /etc/nginx/sites-enabled/default

# Kiểm tra cú pháp
sudo nginx -t

# Reload Nginx
sudo systemctl reload nginx
```

---

### Phần 4 – Cấu hình UFW Firewall

```bash
# Cho phép SSH (giữ kết nối)
sudo ufw allow OpenSSH

# Cho phép cổng cá nhân
sudo ufw allow 8083/tcp

# Bật UFW
sudo ufw enable

# Kiểm tra
sudo ufw status verbose
```

---

### Kết quả

Truy cập website tại: `http://<IP_máy_chủ>:8083`

---

