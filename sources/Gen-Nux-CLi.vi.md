# NUX-Root / General Linux CLi
Ghi chú về việc triển khai dự án với Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](Gen-Nux-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](Gen-Nux-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.vi.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [ENABLE ROOT USER](#enable-root-user)
- [ALLOW SSH FOR ROOT USER](#allow-ssh-for-root-user)
- [REMOVE USER ACCOUNT](#remove-user-account)
- [INSTALL TOOLS](#install-tools)
- [UPDATE NEW PACKAGE](#update-new-package)

---

# ENABLE ROOT USER
>Hệ thống máy chủ Ubuntu mặc định không kích hoạt tài khoản root sau khi cài đặt thành công. Do đó ta kích hoạt tài khoản root như dưới đây.

- Thiết lập mật khẩu cho tài khoản `root`.
```bash
sudo passwd root
```
*Sau đó thực hiện nhập mật khẩu nới cho tài khoản root.*
- Chuyển qua tài khoản `root`.
```bash
sudo -
```

# ALLOW SSH FOR ROOT USER
>Theo mặc định, tài khoản root sau khi được kích hoạt vẫn không được truy cập máy chủ Ubuntu bằng phương thức SSH. Do đó, chúng tôi mở quyền truy cập SSH cho tài khoản root như sau.

- Sửa file `sshd_config`. *(SSH config file)*
```bash
sudo nano /etc/ssh/sshd_config
```
- Cần gỡ bỏ các dấu chú thích (**#**) ở đầu các dòng chỉnh sửa.
- Tại dòng **#PermitRootLogin prohibit-password** hãy sửa thành **PermitRootLogin yes**.
- Tại dòng **#PasswordAuthentication yes** hãy sửa thành **PasswordAuthentication yes**.
- Lưu thay đổi bằng tổ hợp `CTRL + O`. *(over-write)*
- Thoát nano bẳng tổ hợp `CTRL + X`. *(exit)*
- Khởi động lại dịch vụ SSH.
```bash
sudo systemctl restart ssh
```

- Kiểm tra kết nối SSH bằng tài khoản root. 
```bash
sudo ssh root@<"ip_server">
```

# REMOVE USER ACCOUNT
>Theo mặc định, hệ thống máy chủ Ubuntu đã khởi tạo một tài khoản quản trị viên (không phải root) mà bạn đã nhập trong quá trình cài đặt. Do đó, chúng tôi xóa tài khoản quản trị viên như bên dưới.
*Lưu ý: thực hiện thao tác này sau khi kích hoạt thành công tài khoản root.*

- Kiểm tra lại quyền `sudo` của tài khoản cần xóa.
```bash
sudo cat /etc/sudoers.d/<"account_to_be_deleted">
```
*Nếu tài khoản đó có quyền sudo bạn nên xóa file này trước.*
```bash
sudo rm /etc/sudoers.d/<"account_to_be_deleted">
```

- Kiểm tra danh sách người dùng trước khi xóa.
```bash
cat /etc/passwd | grep <"account_to_be_deleted">
```
- Thực hiện xóa tài khoản quản trị.
```bash
userdel -r <"account_to_be_deleted">
```

# INSTALL TOOLS
>Có một số công cụ cần thiết dành cho quản trị viên (theo quan điểm cá nhân) mà nó không được cài đặt mặc định trên máy chủ Ubuntu.

Danh sachs Tools:
- [Net-tools](https://sourceforge.net/projects/net-tools/)
- Telnet Client
- [Tracerout](https://sourceforge.net/projects/traceroute/)
```bash
sudo apt install -y net-tools telnet traceroute
```

# UPDATE NEW PACKAGE
Luôn thực hiện cập nhật bản mới nhất trước khi cài đặt dịch vụ.
```bash
sudo apt update && sudo apt upgrade -y
```

*[Lên đầu trang](#nux-root--general-linux-cli)*