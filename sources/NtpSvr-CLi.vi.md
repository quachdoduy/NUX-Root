# NUX-Root / NTP Server Linux
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](NtpSvr-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](NtpSvr-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Refer to original document](#refer-to-original-document)
    - [Refer to expanded document](#refer-to-expanded-document)
- [BASIC INSTRUCTIONS](#basic-instructions)
    - [Install the NTP Service](#install-the-ntp-service)
    - [Configure NTP Servers](#configure-ntp-servers)
    - [Configure NTP Client](#configure-ntp-client)
    - [Check Status](#check-status)
- [ADVANCED INSTRUCTIONS](#advanced-instructions)

---

<img alt="Keep Alived" src="../assets/images/NtpSvr-CLi.png">

# PREFACE
NTP (Network Time Protocol) là một giao thức được sử dụng để đồng bộ hóa tất cả các đồng hồ hệ thống trong mạng để sử dụng cùng một thời gian.
NTP thuộc bộ giao thức TCP/IP truyền thống và cũng là dịch vụ lâu đời nhất trong nền tảng khoa học máy tính.

- So sánh các phiên bản NTP Server trên Linux: Chrony, NTPsec và NTP.

| **Tính năng** | **NTP (Classic NTPd)** | **Chrony** | **NTPsec** |
|:---|:---|:---|:---|
| Phát triển bởi | Network Time Foundation | Red Hat | NTPsec Project |
| Mục tiêu chính | Dịch vụ NTP truyền thống, hỗ trợ đầy đủ các tính năng đồng bộ hóa thời gian. | Tối ưu cho môi trường không liên tục, có thể hoạt động tốt với đồng hồ hệ thống không ổn định. | Phiên bản bảo mật cao, nhẹ hơn NTP truyền thống. |
| Bảo mật | Bảo mật kém hơn do có lịch sử lâu đời và nhiều lỗ hổng bảo mật. | Có các cải tiến bảo mật nhưng không tập trung mạnh vào bảo mật. | Được thiết kế đặc biệt để loại bỏ các lỗ hổng bảo mật trong NTP truyền thống. |
| Hiệu suất | Hoạt động tốt nhưng có độ trễ lớn hơn trong các hệ thống di động hoặc mạng không ổn định. | Hiệu suất cao hơn, có thể đồng bộ nhanh hơn trong môi trường thay đổi liên tục. | Nhẹ hơn NTPd, cải thiện hiệu suất và giảm độ trễ. |
| Sử dụng trong thực tế | Thích hợp cho các hệ thống yêu cầu đồng bộ hóa chính xác nhưng không cần tối ưu hiệu suất. | Tốt hơn cho các hệ thống di động, máy chủ bị mất kết nối mạng tạm thời. | Tốt cho các hệ thống cần bảo mật cao. |
| Cấu hình đơn giản | Khó cấu hình hơn so với Chrony. | Cấu hình đơn giản, dễ tinh chỉnh. | Cấu hình gần giống NTP truyền thống nhưng tập trung vào bảo mật. |
| Hỗ trợ giao thức NTS (Network Time Security) | Không hỗ trợ. | Không hỗ trợ. | Có hỗ trợ. |

## Refer to original document
- website: https://www.ntppool.org
- wiki: https://en.wikipedia.org/wiki/Network_Time_Protocol
- rfc-867: https://datatracker.ietf.org/doc/html/rfc867
- rfc-868: https://datatracker.ietf.org/doc/html/rfc868

## Refer to expanded document
- NTP (Classic NTPd)
    * website: https://www.ntp.org
    * manual:  https://doc.ntp.org
- Chrony
    * website: https://chrony.tuxfamily.org
    * manual:  https://chrony.tuxfamily.org/documentation.html
- NTPsec
    * website: https://www.ntpsec.org
    * manual:  https://docs.ntpsec.org

---

# BASIC INSTRUCTIONS
## Install the NTP Service
- Bắt đầu bằng cách cập nhật danh sách gói để đảm bảo bạn có phiên bản kho lưu trữ mới nhất.
```
sudo apt update && sudo apt upgrade -y
```
- Cài đặt gói NTP bằng lệnh sau.
```
sudo apt install -y ntp
```
*Lệnh này cài đặt dịch vụ NTP trên hệ thống Ubuntu của bạn, giúp hệ thống sẵn sàng để cấu hình.*

- Xác minh cài đặt NTP của bạn và kiểm tra số phiên bản bằng cách chạy lệnh sau.
```
sntp --version
```

## Configure NTP Servers
- Cấu hình máy chủ NTP của bạn là một bước quan trọng. Mở tệp cấu hình NTP trong trình soạn thảo văn bản bạn chọn. Ở đây, nano được sử dụng để đơn giản hóa.
```
sudo nano /etc/ntpsec/ntp.conf
```
- Trong tệp cấu hình, hãy thêm hoặc sửa đổi các dòng máy chủ để chỉ định máy chủ NTP ưa thích của bạn. Ví dụ:
<img alt="NTP Config File" src="../assets/images/NtpSvr-CLi_pool.png">

*Lưu ý: Trong ví dụ này sử dụng 2 máy chủ NTP của VNNIC, được công bố trên trang chủ.*

## Configure NTP Client
- Thay đổi múi giờ.
```
sudo timedatectl set-timezone Asia/Ho_Chi_Minh
```

## Check Status
- Kiểm tra dịch vụ NTP.
```
sudo systemctl status ntp
```
- Kiểm tra hàng đợi NTP.
```
ntpq -p
```
- Kiểm tra Ngày giờ.
```
date
```


---

# ADVANCED INSTRUCTIONS

*[Lên đầu trang](#nux-root--ntp-server-linux)*