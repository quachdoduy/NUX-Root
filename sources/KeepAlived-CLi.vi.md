# NUX-Root / KeepAlived Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](KeepAlived-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](KeepAlived-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Tham khảo tài liệu gốc](#tham-khảo-tài-liệu-gốc)
    - [Tham khảo tài liệu mở rộng](#tham-khảo-tài-liệu-mở-rộng)
- [BASIC INSTRUCTIONS](#basic-instructions)
    - [Cài đặt](#cài-đặt)
    - [Kiểm tra trạng thái](#kiểm-tra-trạng-thái)
- [ADVANCED INSTRUCTIONS](#advanced-instructions)


---

# PREFACE
- **Keepalived** là một phần mềm định tuyến được viết bằng C. Mục tiêu chính của dự án này là cung cấp các tiện ích đơn giản và mạnh mẽ để cân bằng tải và tính khả dụng cao cho hệ thống Linux và cơ sở hạ tầng dựa trên Linux. Khung cân bằng tải dựa trên mô-đun hạt nhân Linux Virtual Server (IPVS) nổi tiếng và được sử dụng rộng rãi cung cấp cân bằng tải Layer4. **Keepalived** triển khai một bộ kiểm tra để duy trì và quản lý nhóm máy chủ cân bằng tải một cách năng động và thích ứng theo tình trạng của chúng. Mặt khác, tính khả dụng cao đạt được bằng giao thức VRRP. VRRP là một khối cơ bản để chuyển đổi dự phòng bộ định tuyến. Ngoài ra, **Keepalived** triển khai một bộ móc vào máy trạng thái hữu hạn VRRP cung cấp các tương tác giao thức cấp thấp và tốc độ cao. Để cung cấp khả năng phát hiện lỗi mạng nhanh nhất, **Keepalived** triển khai giao thức BFD. Quá trình chuyển đổi trạng thái VRRP có thể tính đến gợi ý BFD để thúc đẩy quá trình chuyển đổi trạng thái nhanh. Các khung **Keepalived** có thể được sử dụng độc lập hoặc kết hợp với nhau để cung cấp cơ sở hạ tầng phục hồi.

**Keepalived là phần mềm miễn phí**; bạn có thể phân phối lại và/hoặc sửa đổi nó theo các điều khoản của Giấy phép Công cộng GNU do Free Software Foundation công bố; phiên bản 2 của Giấy phép hoặc (tùy bạn lựa chọn) bất kỳ phiên bản nào mới hơn.

## Tham khảo tài liệu gốc
- webpage: https://www.keepalived.org/
- github: https://github.com/acassen/keepalived/
- user guide: https://keepalived.readthedocs.io
- social: https://x.com/keepalived

## Tham khảo tài liệu mở rộng
- RFC 9568: https://datatracker.ietf.org/doc/html/rfc9568

---

# BASIC INSTRUCTIONS

## Cài đặt
- Cài đặt keepalived (cho tất cả các nút trong cụm).
```bash
sudo apt install -y keepalived
```
- Sửa đổi tệp keepalived.conf.
```bash
sudo nano /etc/keepalived/keepalived.conf
```
- Dạng viết tắt như sau.
```bash
vrrp_instance string {          # identify a VRRP instance definition block
    state MASTER|BACKUP         # specify the instance state in standard use
    interface string            # specify the network interface for the instance to run on
    virtual_router_id num       # specify to which VRRP router id the instance belongs
    priority num                # specify the instance priority in the VRRP router (range from 1 to 255)
    advert_int num              # specify the advertisement interval in seconds (set to 1)
    authentication {            # identify a VRRP authentication definition block
        auth_type PASS|AH       # specify which kind of authentication to use (PASS|AH)
        auth_pass string        # specify the password string to use
    }
    virtual_ipaddress {         # identify a VRRP VIP definition block (Block limited to 20 IP addresses) 
        @IP
        @IP
        @IP
    }
}
```
- Ví dụ.
```bash
# *Node Master*
vrrp_instance VipKA {
    state MASTER
    interface ens33
    virtual_router_id 69
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass V3ryS3cr3t
    }
    virtual_ipaddress {
        192.168.69.1
    }
}
```
```bash
# *Node Backup*
vrrp_instance VipKA {
    state BACKUP
    interface ens33
    virtual_router_id 69
    priority 99
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass V3ryS3cr3t
    }
    virtual_ipaddress {
        192.168.69.1
    }
}
```
-  Khởi động lại dịch vụ keepalived.
```bash
sudo systemctl restart keepalived
```

## Kiểm tra trạng thái
- Kiểm tra xem nút nào là nút Master.
```bash
ip addr show | grep "Virtual_IP"
```
- Xem trạng thái VRRP đã được Keepalived.
```bash
watch -n 1 "ip -br addr show dev your_NIC_nae"
```
- Xem nhật ký Keepalived.
```bash
journalctl -u keepalived --no-pager | tail -50
```

# ADVANCED INSTRUCTIONS
- Quá trình bầu chọn Master được chia thành 3 giai đoạn chính, tuân theo các tiêu chuẩn mới nhất hiện hành cho VRRP version 3.
    - Initialize
    - Active
    - Backup

<img alt="Master Election" src="../assets/images/Master Voting Process.png">

*[Lên đầu trang](#nux-root--keepalived-linux-cli)*