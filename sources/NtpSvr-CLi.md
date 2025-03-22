# NUX-Root / NTP Server Linux
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](NtpSvr-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](NtpSvr-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS

---

<img alt="Keep Alived" src="../assets/images/NtpSvr-CLi.png">

# PREFACE
NTP (Network Time Protocol) is a protocol used to synchronize all system clocks in a network to use the same time. 
NTP belongs to the traditional TCP/IP protocol suite and is also the oldest service in the computer science foundation.

- Below are 3 popular NTP services on server systems.
    * NTP
    * Chrony
    * NTPsec

- Comparison of NTP Server versions on Linux: Chrony, NTPsec and NTP.

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

---

# BASIC INSTRUCTIONS
## Install the NTP Service
- Start by updating your package lists to ensure you have the latest version of the repositories.
```
sudo apt update && sudo apt upgrade -y
```
- Install the NTP package using the following command.
```
sudo apt install -y ntp
```
*This command installs the NTP service on your Ubuntu system, making it ready for configuration.*

- Verify your NTP installation and also check the version number by running the following command.
```
sntp --version
```

## Configure NTP Servers
- Configuring your NTP servers is a critical step. Open the NTP configuration file in a text editor of your choice. Here, nano is used for simplicity.
```
sudo nano /etc/ntp.conf
```
- In the configuration file, add or modify the server lines to specify your preferred NTP servers. For example:

## Check Status

---

# ADVANCED INSTRUCTIONS

*[Back to Top](#nux-root--ntp-server-linux)*