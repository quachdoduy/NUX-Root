# NUX-Root / Nettools Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](https://github.com/quachdoduy/NUX-Root/blob/main/sources/Nettools-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](https://github.com/quachdoduy/NUX-Root/blob/main/sources/Nettools-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](https://github.com/quachdoduy/NUX-Root/)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)

# PREFACE
Bộ công cụ **net-tools** là một tập hợp các lệnh quản lý mạng trên hệ điều hành Unix/Linux, được sử dụng rộng rãi trong nhiều năm trước khi bị thay thế bởi **iproute2**.

## Khởi đầu và phát triển ban đầu
- Nguồn gốc: **net-tools** xuất hiện từ thời kỳ đầu của Linux, khoảng những năm 1990.
- Mục đích: Cung cấp các công cụ cơ bản để quản lý mạng, bao gồm cấu hình giao diện mạng (*ifconfig*), kiểm tra kết nối (*netstat*), quản lý bảng định tuyến (*route*), và các lệnh khác.
- Tác giả chính: **Bernd Eckenfels** là người duy trì chính trong giai đoạn sau, nhưng nhiều nhà phát triển khác cũng đã đóng góp.

## Phiên bản và phát triển chính
- Phiên bản ổn định cuối cùng: `net-tools 1.60`, phát hành vào 2001.
- Cập nhật cuối cùng: Một số bản cập nhật nhỏ sau đó xuất hiện trên các bản phân phối Linux, nhưng không có phiên bản chính thức mới nào.

## Ngừng phát triển và thay thế
- Do bộ **net-tools** đã cũ và không hỗ trợ tốt các tính năng mạng hiện đại như *IPv6*, *policy routing*, và *network namespaces*, nó bắt đầu bị thay thế bởi bộ công cụ **iproute2** từ cuối những năm 2000.
- Các bản phân phối Linux như `Debian`, `Ubuntu`, và `RHEL` đã dần **loại bỏ net-tools** khỏi cài đặt mặc định.

## Công cụ thay thế hiện đại
- Bộ **iproute2** được phát triển để thay thế **net-tools**, với các lệnh mới:
    - `ip addr` thay thế `ifconfig`
    - `ip route` thay thế `route`
    - `ss` thay thế `netstat`
    - `ip neighbor` thay thế `arp`

## Tình trạng hiện tại
- Dù không còn được duy trì tích cực, **net-tools** vẫn có mặt trong một số hệ thống cũ hoặc phiên bản Linux nhẹ.
- Một số bản fork tồn tại trên GitHub, nhưng không được cập nhật chính thức.
- Người dùng hiện đại được khuyến nghị sử dụng iproute2 thay vì net-tools.

## Tham khảo tài liệu gốc
- SourceForge (lưu trữ gốc của net-tools): https://sourceforge.net/projects/net-tools/
- GitHub của Bernd Eckenfels: https://github.com/ecki/net-tools
- Trang Linux Kernel Archives: https://www.kernel.org/

# COMMONLY USED COMMANDS

## ifconfig
- Tóm tắt:
    `ifconfig [-v] [-a] [-s] [interface]`
    `ifconfig [-v] interface [aftype] options | address ...`
- Miêu tả:
    - Ifconfig được sử dụng để cấu hình giao diện mạng trú ngụ trong nhân. Nó được sử dụng tại thời điểm khởi động để thiết lập giao diện khi cần thiết. Sau đó, nó thường chỉ cần thiết khi gỡ lỗi hoặc khi cần điều chỉnh hệ thống.
    - Nếu không có đối số nào được đưa ra, ifconfig sẽ hiển thị trạng thái của các giao diện đang hoạt động. Nếu chỉ có một đối số giao diện, nó sẽ chỉ hiển thị trạng thái của giao diện đã cho; nếu chỉ có một đối số -a, nó sẽ hiển thị trạng thái của tất cả các giao diện, ngay cả những giao diện đã ngừng hoạt động. Nếu không, nó sẽ cấu hình một giao diện.
- Tùy chọn:
    `-a` : hiển thị tất cả các giao diện hiện có, ngay cả khi đã ngừng hoạt động.
    `-s` : hiển thị danh sách ngắn (như `netstat -i`).
    `-v` : sẽ chi tiết hơn đối với một số điều kiện lỗi.
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Ví dụ:
    `ifconfig` : Hiển thị tất cả các giao diện mạng đang hoạt động (có địa chỉ IP).
    `ifconfig -a` : Hiển thị tất cả các giao diện mạng bao gồm cả các giao diện không hoạt động.
    `ifconfig eth0 192.168.1.100 netmask 255.255.255.0` : Gán địa chỉ IP `192.168.1.100` với mặt nạ mạng con `255.255.255.0` cho giao diện `eth0`.
    `ifconfig eth0 up` : Bật giao diện mạng.
    `ifconfig eth0 down` : Vô hiệu hóa giao diện mạng.

## arp
- Tóm tắt:
    `arp [-vn] [-H type] [-i if] [-ae] [hostname]`
    `arp [-v] [-i if] -d hostname [pub]`
    `arp [-v] [-H type] [-i if] -s hostname hw_addr [temp]`
    `arp [-v] [-H type] [-i if] -s hostname hw_addr [netmask nm] pub`
    `arp [-v] [-H type] [-i if] -Ds hostname ifname [netmask nm] pub`
    `arp [-vnD] [-H type] [-i if] -f [filename]`
- Miêu tả:
    - Arp manipulates or displays the kernel's IPv4 network neighbour cache. It can add entries to the table, delete one or display the current content.
    - ARP stands for Address Resolution Protocol, which is used to find the media access control address of a network neighbour for a given IPv4 Address.
- Chế độ:
    - **arp** with no mode specifier will print the current content of the table. It is possible to limit the number of entries printed, by specifying an hardware address type, interface name or host address.
    - **arp -d** *address* will delete a ARP table entry. Root or netadmin privilege is required to do this. The entry is found by IP address. If a hostname is given, it will be resolved before looking up the entry in the ARP table.
    - **arp -s** *address hw_addr* is used to set up a new table entry. The format of the hw_addr parameter is dependent on the hardware class, but for most classes one can assume that the usual presentation can be used. For the Ethernet class, this is 6 bytes in hexadecimal, separated by colons. When adding proxy arp entries (that is those with the publish flag set) a netmask may be specified to proxy arp for entire subnets. This is not good practice, but is supported by older kernels because it can be useful. If the temp flag is not supplied entries will be permanent stored into the ARP cache. To simplify setting up entries for one of your own network interfaces, you can use the arp -Ds address ifname form. In that case the hardware address is taken from the interface with the specified name.
- Tùy chọn:
    `-v` or `--verbose` : Tell the user what is going on by being verbose.
    `-n` or `--numeric` : Shows numerical addresses instead of trying to determine symbolic host, port or user names.
    `-D` or `--use-device` : Instead of a hw_addr, the given argument is the name of an interface. arp will use the MAC address of that interface for the table entry. This is usually the best option to set up a proxy ARP entry to yourself.
    *For more, please refer to the original document.*
- Ví dụ:
    `arp -a` : Displays all entries in the ARP table, including IP address, MAC address, and status.
    `arp -n` : Show ARP table without host name resolution, only show IP and MAC addresses.
    `arp -d 192.168.1.1` : Remove IP address 192.168.1.1 from the ARP table.
    `arp -i eth0 -Ds 10.0.0.2 eth1 pub` : This will answer ARP requests for 10.0.0.2 on eth0 with the MAC address for eth1. 
    `/usr/sbin/arp -i eth1 -d 10.0.0.1` : Delete the ARP table entry for 10.0.0.1 on interface eth1. This will match published proxy ARP entries and permanent entries.

## route

## netstat

*[Lên đầu trang](#nux-root--nettools-linux-cli)*