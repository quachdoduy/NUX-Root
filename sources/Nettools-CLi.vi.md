# NUX-Root / Nettools Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](Nettools-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](Nettools-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.vi.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Khởi đầu và phát triển ban đầu](#khởi-đầu-và-phát-triển-ban-đầu)
    - [Phiên bản và phát triển chính](#phiên-bản-và-phát-triển-chính)
    - [Ngừng phát triển và thay thế](#ngừng-phát-triển-và-thay-thế)
    - [Công cụ thay thế hiện đại](#công-cụ-thay-thế-hiện-đại)
    - [Tình trạng hiện tại](#tình-trạng-hiện-tại)
    - [Tham khảo tài liệu gốc](#tham-khảo-tài-liệu-gốc)
- [COMMONLY USED COMMANDS](#commonly-used-commands)
    - [ifconfig](#ifconfig)
    - [arp](#arp)
    - [route](#route)
    - [netstat](#netstat)

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
- Tóm tắt:<br>
    `ifconfig [-v] [-a] [-s] [interface]`<br>
    `ifconfig [-v] interface [aftype] options | address ...`
- Miêu tả:
    - Ifconfig được sử dụng để cấu hình giao diện mạng trú ngụ trong nhân. Nó được sử dụng tại thời điểm khởi động để thiết lập giao diện khi cần thiết. Sau đó, nó thường chỉ cần thiết khi gỡ lỗi hoặc khi cần điều chỉnh hệ thống.
    - Nếu không có đối số nào được đưa ra, ifconfig sẽ hiển thị trạng thái của các giao diện đang hoạt động. Nếu chỉ có một đối số giao diện, nó sẽ chỉ hiển thị trạng thái của giao diện đã cho; nếu chỉ có một đối số -a, nó sẽ hiển thị trạng thái của tất cả các giao diện, ngay cả những giao diện đã ngừng hoạt động. Nếu không, nó sẽ cấu hình một giao diện.
- Tùy chọn:<br>
    `-a` : hiển thị tất cả các giao diện hiện có, ngay cả khi đã ngừng hoạt động.<br>
    `-s` : hiển thị danh sách ngắn (như `netstat -i`).<br>
    `-v` : sẽ chi tiết hơn đối với một số điều kiện lỗi.<br>
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Ví dụ:<br>
    `ifconfig` : Hiển thị tất cả các giao diện mạng đang hoạt động (có địa chỉ IP).<br>
    `ifconfig -a` : Hiển thị tất cả các giao diện mạng bao gồm cả các giao diện không hoạt động.<br>
    `ifconfig eth0 192.168.1.100 netmask 255.255.255.0` : Gán địa chỉ IP `192.168.1.100` với mặt nạ mạng con `255.255.255.0` cho giao diện `eth0`.<br>
    `ifconfig eth0 up` : Bật giao diện mạng.<br>
    `ifconfig eth0 down` : Vô hiệu hóa giao diện mạng.

## arp
- Tóm tắt:<br>
    `arp [-vn] [-H type] [-i if] [-ae] [hostname]`<br>
    `arp [-v] [-i if] -d hostname [pub]`<br>
    `arp [-v] [-H type] [-i if] -s hostname hw_addr [temp]`<br>
    `arp [-v] [-H type] [-i if] -s hostname hw_addr [netmask nm] pub`<br>
    `arp [-v] [-H type] [-i if] -Ds hostname ifname [netmask nm] pub`<br>
    `arp [-vnD] [-H type] [-i if] -f [filename]`
- Miêu tả:
    - Arp thao tác hoặc hiển thị bộ nhớ đệm mạng lân cận IPv4 của hạt nhân. Nó có thể thêm mục vào bảng, xóa một mục hoặc hiển thị nội dung hiện tại.
    - ARP là viết tắt của Address Resolution Protocol, được sử dụng để tìm địa chỉ kiểm soát truy cập phương tiện của mạng lân cận cho một Địa chỉ IPv4 nhất định.
- Chế độ:
    - **arp** không có chỉ định chế độ sẽ in nội dung hiện tại của bảng. Có thể giới hạn số mục được in bằng cách chỉ định loại địa chỉ phần cứng, tên giao diện hoặc địa chỉ máy chủ.
    - **arp -d** *address* sẽ xóa 1 mục trong bảng ARP. Cần có quyền root hoặc netadmin để thực hiện việc này. Mục được tìm thấy theo địa chỉ IP. Nếu tên máy chủ được cung cấp, nó sẽ được giải quyết trước khi tra cứu mục trong bảng ARP.
    - **arp -s** *address hw_addr* được sử dụng để thiết lập một mục bảng mới. Định dạng của tham số hw_addr phụ thuộc vào lớp phần cứng, nhưng đối với hầu hết các lớp, người ta có thể cho rằng có thể sử dụng cách trình bày thông thường. Đối với lớp Ethernet, đây là 6 byte ở dạng thập lục phân, được phân tách bằng dấu hai chấm. Khi thêm các mục proxy arp (tức là các mục có cờ publish được đặt), có thể chỉ định một netmask cho proxy arp cho toàn bộ các mạng con. Đây không phải là cách làm tốt, nhưng được các kernel cũ hơn hỗ trợ vì nó có thể hữu ích. Nếu cờ temp không được cung cấp, các mục sẽ được lưu trữ cố định vào bộ đệm ARP. Để đơn giản hóa việc thiết lập các mục cho một trong các giao diện mạng của riêng bạn, bạn có thể sử dụng biểu mẫu arp -Ds address ifname. Trong trường hợp đó, địa chỉ phần cứng được lấy từ giao diện có tên đã chỉ định.
- Tùy chọn:<br>
    `-v` hoặc `--verbose` : Hiển thị rõ ràng cho người dùng biết chuyện gì đang xảy ra.<br>
    `-n` hoặc `--numeric` : Hiển thị địa chỉ số thay vì cố gắng xác định tên máy chủ, cổng hoặc tên người dùng tượng trưng.<br>
    `-D` hoặc `--use-device` : Thay vì hw_addr, đối số được đưa ra là tên của một giao diện. arp sẽ sử dụng địa chỉ MAC của giao diện đó cho mục nhập bảng. Đây thường là tùy chọn tốt nhất để thiết lập mục nhập ARP proxy cho chính bạn.<br>
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Ví dụ:<br>
    `arp -a` : Hiển thị tất cả các mục trong bảng ARP, bao gồm địa chỉ IP, địa chỉ MAC và trạng thái.<br>
    `arp -n` : Hiển thị bảng ARP mà không phân giải tên máy chủ, chỉ hiển thị địa chỉ IP và MAC.<br>
    `arp -d 192.168.1.1` : Xóa địa chỉ IP `192.168.1.1` khỏi bảng ARP.<br>
    `arp -i eth0 -Ds 10.0.0.2 eth1 pub` : Điều này sẽ trả lời các yêu cầu ARP cho `10.0.0.2` trên `eth0` với địa chỉ MAC cho `eth1`.<br>
    `/usr/sbin/arp -i eth1 -d 10.0.0.1` : Xóa mục nhập bảng ARP cho `10.0.0.1` trên giao diện `eth1`. Điều này sẽ khớp với các mục nhập ARP proxy đã xuất bản và các mục nhập cố định.

## route
- Tóm tắt:<br>
    `route [-CFvnNee] [-A family |-4|-6]`
- Miêu tả:
    - **Route** thao tác các bảng định tuyến IP của hạt nhân. Công dụng chính của nó là thiết lập các tuyến tĩnh đến các máy chủ hoặc mạng cụ thể thông qua một giao diện sau khi đã được cấu hình bằng chương trình [ifconfig](#ifconfig)(8).
    - Khi tùy chọn **add** hoặc **del** được sử dụng, **route** sẽ sửa đổi các bảng định tuyến. Nếu không có các tùy chọn này, **route** sẽ hiển thị nội dung hiện tại của các bảng định tuyến.
- Tùy chọn:<br>
    `-A family` : sử dụng họ địa chỉ được chỉ định (ví dụ `inet`). Sử dụng **route --help** để có danh sách đầy đủ. Bạn có thể sử dụng **-6** làm bí danh cho **--inet6** và **-4** làm bí danh cho **-A inet**.<br>
    `-F` : hoạt động trên bảng định tuyến FIB (Cơ sở thông tin chuyển tiếp) của hạt nhân. Đây là mặc định.<br>
    `-C` : hoạt động trên bộ đệm định tuyến của hạt nhân.<br>
    `-n` : hiển thị địa chỉ số thay vì cố gắng xác định tên máy chủ tượng trưng. Điều này hữu ích nếu bạn đang cố gắng xác định lý do tại sao tuyến đường đến máy chủ tên của bạn đã biến mất.<br>
    `del` : xóa một tuyến đường.<br>
    `add` : thêm một tuyến đường mới.<br>
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Ví dụ:<br>
    `route add -net 127.0.0.0 netmask 255.0.0.0 dev lo` : thêm mục **loopback** bình thường, sử dụng netmask **255.0.0.0** và liên kết với thiết bị "**lo**".<br>
    `route add -net 192.56.76.0 netmask 255.255.255.0 dev eth0` : thêm một tuyến đường đến mạng cục bộ **192.56.76.x** qua "**eth0**". Từ "**dev**" có thể được bỏ qua ở đây.<br>
    `route add -net 224.0.0.0 netmask 240.0.0.0 dev eth0` : Đây là một tài liệu mơ hồ để mọi người biết cách thực hiện. Điều này thiết lập tất cả các tuyến IP **class D (multicast)*** đi qua "**eth0**". Đây là dòng cấu hình bình thường chính xác với một hạt nhân đa hướng.

## netstat
- Lưu ý:
    *Chương trình này đã lỗi thời.*
    - Thay thế cho **netstat** là **ss**.
    - Thay thế cho **netstat -r** là **ip route**.
    - Thay thế cho **netstat -i** là **ip -s link**.
    - Thay thế cho **netstat -g** là **ip maddr**.
- Tóm tắt:<br>
    `netstat [*address_family_options*] [--tcp|-t] [--udp|-u] [--udplite|-U] [--raw|-w] [--listening|-l] [--all|-a] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--symbolic|-N] [--extend|-e[--extend|-e]] [--timers|-o] [--program|-p] [--verbose|-v] [--continuous|-c] [--wide|-W] netstat {--route|-r} [address_family_options] [--extend|-e[--extend|-e]] [--verbose|-v] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--interfaces|-i} [--all|-a] [--extend|-e[--extend|-e]] [--verbose|-v] [--program|-p] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--groups|-g} [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--masquerade|-M} [--extend|-e] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--statistics|-s} [--tcp|-t] [--udp|-u] [--udplite|-U] [--raw|-w] netstat {--version|-V} netstat {--help|-h} *address_family_options*:`<br>
    `[-4|--inet] [-6|--inet6] [--protocol={inet,inet6,unix,ipx,ax25,netrom,ddp, ... } ] [--unix|-x] [--inet|--ip|--tcpip] [--ax25] [--x25] [--rose] [--ash] [--ipx] [--netrom] [--ddp|--appletalk] [--econet|--ec]`
- Miêu tả:
    - **Netstat** in thông tin về hệ thống mạng Linux.
    - Loại thông tin được in được kiểm soát bởi đối số đầu tiên, như sau:<br>
        `-r` hoặc `--route` : Hiển thị bảng định tuyến hạt nhân.<br>
        `-g` hoặc `--groups` : Hiển thị thông tin thành viên nhóm đa hướng cho IPv4 và IPv6.<br>
        `-i` hoặc `--interfaces` : Hiển thị bảng tất cả các giao diện mạng.<br>
        `-M` hoặc `--masquerade` : Hiển thị danh sách các kết nối được ngụy trang.<br>
        `-s` hoặc `--statistics` : Hiển thị số liệu thống kê tóm tắt cho từng giao thức.
- Tùy chọn:<br>
    `-v` or `--verbose` : Nói cho người dùng biết những gì đang diễn ra bằng cách nói dài dòng. Đặc biệt là in một số thông tin hữu ích về các họ địa chỉ chưa được cấu hình.<br>
    `-W` or `--wide` : Không cắt bớt địa chỉ IP bằng cách sử dụng đầu ra rộng như cần thiết. Hiện tại, điều này là tùy chọn để không làm hỏng các tập lệnh hiện có.<br>
    `-n` or `--numeric` : Hiển thị địa chỉ số thay vì cố gắng xác định tên máy chủ, cổng hoặc tên người dùng tượng trưng.<br>
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Ví dụ:<br>
    `netstat -a` : Hiển thị tất cả các socket đang hoạt động (TCP/UDP), bao gồm cả các cổng đang lắng nghe (`LISTEN`).<br>
    `netstat -l` : Hiển thị danh sách các cổng mà hệ thống đang lắng nghe.<br>
    `netstat -an | grep ":22"` : Kiểm tra kết nối trên cổng `22` (SSH).<br>
    `netstat -utnpl` : Câu lệnh tôi thích nhất.

*[Lên đầu trang](#nux-root--nettools-linux-cli)*