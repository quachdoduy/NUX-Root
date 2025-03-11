# NUX-Root / Nettools Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](Nettools-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](Nettools-CLi.vi.md)
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
    - Arp thao tác hoặc hiển thị bộ nhớ đệm mạng lân cận IPv4 của hạt nhân. Nó có thể thêm mục vào bảng, xóa một mục hoặc hiển thị nội dung hiện tại.
    - ARP là viết tắt của Address Resolution Protocol, được sử dụng để tìm địa chỉ kiểm soát truy cập phương tiện của mạng lân cận cho một Địa chỉ IPv4 nhất định.
- Chế độ:
    - **arp** không có chỉ định chế độ sẽ in nội dung hiện tại của bảng. Có thể giới hạn số mục được in bằng cách chỉ định loại địa chỉ phần cứng, tên giao diện hoặc địa chỉ máy chủ.
    - **arp -d** *address* sẽ xóa 1 mục trong bảng ARP. Cần có quyền root hoặc netadmin để thực hiện việc này. Mục được tìm thấy theo địa chỉ IP. Nếu tên máy chủ được cung cấp, nó sẽ được giải quyết trước khi tra cứu mục trong bảng ARP.
    - **arp -s** *address hw_addr* được sử dụng để thiết lập một mục bảng mới. Định dạng của tham số hw_addr phụ thuộc vào lớp phần cứng, nhưng đối với hầu hết các lớp, người ta có thể cho rằng có thể sử dụng cách trình bày thông thường. Đối với lớp Ethernet, đây là 6 byte ở dạng thập lục phân, được phân tách bằng dấu hai chấm. Khi thêm các mục proxy arp (tức là các mục có cờ publish được đặt), có thể chỉ định một netmask cho proxy arp cho toàn bộ các mạng con. Đây không phải là cách làm tốt, nhưng được các kernel cũ hơn hỗ trợ vì nó có thể hữu ích. Nếu cờ temp không được cung cấp, các mục sẽ được lưu trữ cố định vào bộ đệm ARP. Để đơn giản hóa việc thiết lập các mục cho một trong các giao diện mạng của riêng bạn, bạn có thể sử dụng biểu mẫu arp -Ds address ifname. Trong trường hợp đó, địa chỉ phần cứng được lấy từ giao diện có tên đã chỉ định.
- Tùy chọn:
    `-v` hoặc `--verbose` : Hiển thị rõ ràng cho người dùng biết chuyện gì đang xảy ra.
    `-n` hoặc `--numeric` : Hiển thị địa chỉ số thay vì cố gắng xác định tên máy chủ, cổng hoặc tên người dùng tượng trưng.
    `-D` hoặc `--use-device` : Thay vì hw_addr, đối số được đưa ra là tên của một giao diện. arp sẽ sử dụng địa chỉ MAC của giao diện đó cho mục nhập bảng. Đây thường là tùy chọn tốt nhất để thiết lập mục nhập ARP proxy cho chính bạn.
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Ví dụ:
    `arp -a` : Hiển thị tất cả các mục trong bảng ARP, bao gồm địa chỉ IP, địa chỉ MAC và trạng thái.
    `arp -n` : Hiển thị bảng ARP mà không phân giải tên máy chủ, chỉ hiển thị địa chỉ IP và MAC.
    `arp -d 192.168.1.1` : Xóa địa chỉ IP `192.168.1.1` khỏi bảng ARP.
    `arp -i eth0 -Ds 10.0.0.2 eth1 pub` : Điều này sẽ trả lời các yêu cầu ARP cho `10.0.0.2` trên `eth0` với địa chỉ MAC cho `eth1`.
    `/usr/sbin/arp -i eth1 -d 10.0.0.1` : Xóa mục nhập bảng ARP cho `10.0.0.1` trên giao diện `eth1`. Điều này sẽ khớp với các mục nhập ARP proxy đã xuất bản và các mục nhập cố định.

## route

## netstat

*[Lên đầu trang](#nux-root--nettools-linux-cli)*