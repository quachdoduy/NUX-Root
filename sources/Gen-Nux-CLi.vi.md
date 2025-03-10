# NUX-Root / General Linux CLi
Ghi chú về việc triển khai dự án với Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](https://github.com/quachdoduy/NUX-Root/blob/main/README.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](https://github.com/quachdoduy/NUX-Root/blob/main/README.vi.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [Table of Contents](#nux-root--general-linux-cli)
- [General]()

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


*[Lên đầu trang](#nux-root--general-linux-cli)*