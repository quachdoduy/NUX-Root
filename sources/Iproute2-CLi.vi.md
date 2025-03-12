# NUX-Root / Iproute2 Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](Iproute2-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](Iproute2-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Tại sao lại là iproute2?](#tại-sao-lại-là-iproute2)
    - [Lịch sử](#lịch-sử)
    - [Các công nghệ quan trọng mà iproute2 hỗ trợ](#các-công-nghệ-quan-trọng-mà-iproute2-hỗ-trợ)
- [COMMONLY USED COMMANDS](#commonly-used-commands)
    - [ip link](#ip-link)
    - [ip addr](#ip-addr)
    - [ip route](#ip-route)
    - [ip rule](#ip-rule)
    - [ip neigh](#ip-neigh)
    - [ip monitor](#ip-monitor)

# PREFACE
**iproute2** là công cụ hiện đại, thay thế hoàn toàn **net-tools** với nhiều cải tiến về hiệu suất, bảo mật và hỗ trợ IPv6. Nếu bạn đã quen với **net-tools**, việc chuyển sang **iproute2** sẽ giúp bạn quản lý hệ thống mạng Linux hiệu quả hơn.

## Tại sao lại là iproute2?
- Hầu hết các bản phân phối Linux và hầu hết UNIX hiện đang sử dụng các lệnh `arp`, `ifconfig` và `route` đáng kính. Mặc dù các công cụ này hoạt động, nhưng chúng cho thấy một số hành vi không mong muốn trong Linux 2.2 trở lên. Ví dụ, đường hầm GRE là một phần không thể thiếu của định tuyến ngày nay, nhưng yêu cầu các công cụ hoàn toàn khác.
- Với iproute2, đường hầm là một phần không thể thiếu của bộ công cụ.
- Các hạt nhân Linux 2.2 trở lên bao gồm một hệ thống con mạng được thiết kế lại hoàn toàn. Mã mạng mới này mang lại hiệu suất Linux và một bộ tính năng có ít đối thủ cạnh tranh trong lĩnh vực hệ điều hành nói chung. Trên thực tế, mã định tuyến, lọc và phân loại mới có nhiều tính năng hơn mã do nhiều bộ định tuyến, tường lửa và sản phẩm định hình lưu lượng chuyên dụng cung cấp.
- Khi các khái niệm mạng mới được phát minh, mọi người đã tìm ra cách để dán chúng lên trên khuôn khổ hiện có trong các hệ điều hành hiện có. Việc liên tục phân lớp các phần tử thừa này đã dẫn đến mã mạng chứa đầy hành vi kỳ lạ, giống như hầu hết các ngôn ngữ của con người. Trước đây, Linux đã mô phỏng cách xử lý nhiều thứ này của SunOS, điều này không lý tưởng.
- Khung làm việc mới này giúp thể hiện rõ ràng các tính năng trước đây nằm ngoài khả năng của Linux.

## Lịch sử
- Khoảng cuối những năm 1990, **Alexey Kuznetsov**, một nhà phát triển kernel của Linux, đã tạo ra `iproute2` để thay thế `net-tools`. Công cụ này được thiết kế để làm việc với Linux Kernel 2.2 trở lên, hỗ trợ tốt hơn cho các tính năng mạng mới.

| Năm | Sự kiện |
|-------|------------------------------------------------------------------------------------|
| 1999  | `iproute2` ra đời, thay thế dần `net-tools`. Hỗ trợ IPv4, định tuyến nâng cao. |
| 2002  | Linux Kernel 2.4 cải tiến nhiều tính năng networking, iproute2 tiếp tục được mở rộng. |
| 2003-2005 | Hỗ trợ mạnh hơn cho IPv6, **`iproute2` trở thành công cụ mặc định trên các bản phân phối Linux mới.** |
| 2007-2010 | Linux Kernel 2.6 ra mắt với cải tiến lớn về networking, `iproute2` tiếp tục phát triển để hỗ trợ Netlink API. |
| 2015-2018 | `iproute2` iproute2 được cập nhật để hỗ trợ `network namespaces`, `container networking` (*dành cho Docker, Kubernetes*). |
| 2020 - nay | Tiếp tục phát triển, hỗ trợ XDP, eBPF, Traffic Control (QoS) và các công nghệ mới trong Linux Kernel. |

## Các công nghệ quan trọng mà iproute2 hỗ trợ
1. **IPv6** – Hỗ trợ đầy đủ, trong khi `net-tools` chỉ có hỗ trợ hạn chế.
2. **Netlink API** – Giao tiếp trực tiếp với Kernel, giúp lệnh ip nhanh hơn so với `ifconfig`.
3. **Policy-Based Routing** – Định tuyến dựa trên chính sách (ip rule), hỗ trợ nhiều bảng định tuyến (ip route).
4. **Traffic Control (QoS)** – Điều khiển băng thông và quản lý hàng đợi (`tc`).
5. **Network Namespace** – Hỗ trợ container networking (Docker, Kubernetes).

## Tham khảo tài liệu gốc
- **Mã nguồn iproute2 trên kernel.org**: https://www.kernel.org/pub/linux/utils/net/iproute2/
- **Kho lưu trữ Git chính thức**:  https://git.kernel.org/pub/scm/network/iproute2/iproute2.git
- **Trang manual (man pages) của iproute2**: https://man7.org/linux/man-pages/man8/
- **Linux Foundation Networking Guide**: https://wiki.linuxfoundation.org/networking/iproute2
- **Arch Linux Wiki về iproute2 (rất chi tiết)**: https://wiki.archlinux.org/title/Iproute2
- **Debian Wiki về iproute2**: https://wiki.debian.org/IPRoute2

# COMMONLY USED COMMANDS

![Overview](../assets/images/IPRoute2.png "Overview")

## ip link
- Synopsis:<br>
    `ip link [help | show | add | delete | set]`<br>
- Description:
    - Lệnh `ip link` trong `iproute2` được sử dụng để quản lý giao diện mạng. Nó cho phép hiển thị thông tin, bật/tắt, đổi tên, cấu hình MTU và thay đổi địa chỉ MAC của card mạng.
- Parameter:<br>
    `help` : hiển thị trợ giúp.<br>
    `show` : liệt kê tất cả các giao diện mạng.<br>
    `add` : thêm giao diện mạng.<br>
    `delete` : xóa giao diện mạng.<br>
    `set` : thiết lập giao diện mạng.<br>
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Examples:
    - `ip link show` : Xem danh sách giao diện mạng.
    - `ip link show dev eth0` : Liệt kê một giao diện cụ thể.
    - `ip -brief link show` : Hiển thị thông tin dưới dạng ngắn gọn.
    - `ip link set eth0 up` : Bật (enable) giao diện mạng eth0.
    - `ip link set eth0 down` : Tắt (disable) giao diện mạng eth0.
    - `ip link set eth0 name wan0` : Đổi tên giao diện mạng eth0 thành wan0.
    - `ip link set eth0 down && ip link set eth0 up` : Khởi động lại giao diện mạng.

## ip addr
- Synopsis:<br>
    `ip addr [help | show | add | del | change | replace | flush]`<br>
    *Chú ý: câu lệnh đầy đủ là `ip address` nhưng có thể viết tắt là `ip addr`.*
- Description:
    - Lệnh `ip addr` trong `iproute2` được sử dụng để quản lý địa chỉ IP trên các giao diện mạng trong Linux. Nó có thể hiển thị, thêm, xóa và thay đổi địa chỉ IP, cũng như cấu hình các tham số liên quan như broadcast, netmask, và scope.
- Parameter:<br>
    `help` : hiển thị trợ giúp.<br>
    `show` : hiển thị địa chỉ IP trên hệ thống.<br>
    `add` : thêm địa chỉ IP vào giao diện mạng.<br>
    `del` : xóa địa chỉ IP khỏi giao diện.<br>
    *Để biết thêm thông tin, vui lòng tham khảo tài liệu gốc.*
- Examples:
    - `ip addr show` : Liệt kê tất cả các địa chỉ IP trên tất cả các giao diện.
    - `ip addr show dev eth0` :  Liệt kê địa chỉ IP của một giao diện cụ thể (eth0).
    - `ip -brief addr show` : Hiển thị địa chỉ IP dưới dạng ngắn gọn.
    - `ip addr add 192.168.1.100/24 dev eth0` : Thêm địa chỉ IPv4 vào giao diện eth0.
    - `ip addr add 192.168.1.100/24 brd 192.168.1.255 dev eth0` : Thêm địa chỉ IP với broadcast cụ thể.
    - `ip addr del 192.168.1.100/24 dev eth0` : Xóa địa chỉ IPv4 khỏi eth0.
    - `ip addr flush dev eth0` : Xóa toàn bộ địa chỉ IP trên eth0.
    - `ip addr show | grep dynamic` : Xem địa chỉ được cấp bởi DHCP.
    - `ip addr add 192.168.2.100/24 dev eth0 label eth0:1` : Thêm IP alias trên eth0:1.

## ip route
- Description:
    - Lệnh `ip route` được sử dụng để quản lý bảng định tuyến trong Linux. Nó cho phép hiển thị, thêm, xóa và thay đổi các tuyến đường (routes) trong hệ thống mạng.
- Usage:
    - `ip route { list | flush } SELECTOR`
    - `ip route save SELECTOR`
    - `ip route restore`
    - `ip route showdump`
    - `ip route get [ ROUTE_GET_FLAGS ] ADDRESS [ from ADDRESS iif STRING ] [ oif STRING ] [ tos TOS ] [ mark NUMBER ] [ vrf NAME ] [ uid NUMBER ] [ ipproto PROTOCOL ] [ sport NUMBER ] [ dport NUMBER ]`
    - `ip route { add | del | change | append | replace } ROUTE`
- Parameter:
    - SELECTOR := [ root PREFIX ] [ match PREFIX ] [ exact PREFIX ] [ table TABLE_ID ] [ vrf NAME ] [ proto RTPROTO ] [ type TYPE ] [ scope SCOPE ]
    - ROUTE := NODE_SPEC [ INFO_SPEC ]
    - NODE_SPEC := [ TYPE ] PREFIX [ tos TOS ] [ table TABLE_ID ] [ proto RTPROTO ] [ scope SCOPE ] [ metric METRIC ] [ ttl-propagate { enabled | disabled } ]
    - INFO_SPEC := { NH | nhid ID } OPTIONS FLAGS [ nexthop NH ]...
    - NH := [ encap ENCAPTYPE ENCAPHDR ] [ via [ FAMILY ] ADDRESS ] [ dev STRING ] [ weight NUMBER ] NHFLAGS
    - FAMILY := [ inet | inet6 | mpls | bridge | link ]
    - OPTIONS := FLAGS [ mtu NUMBER ] [ advmss NUMBER ] [ as [ to ] ADDRESS ] [ rtt TIME ] [ rttvar TIME ] [ reordering NUMBER ] [ window NUMBER ] [ cwnd NUMBER ] [ initcwnd NUMBER ] [ ssthresh NUMBER ] [ realms REALM ] [ src ADDRESS ] [ rto_min TIME ] [ hoplimit NUMBER ] [ initrwnd NUMBER ] [ features FEATURES ] [ quickack BOOL ] [ congctl NAME ] [ pref PREF ] [ expires TIME ] [ fastopen_no_cookie BOOL ]
    - TYPE := { unicast | local | broadcast | multicast | throw | unreachable | prohibit | blackhole | nat }
    - TABLE_ID := [ local | main | default | all | NUMBER ]
    - SCOPE := [ host | link | global | NUMBER ]
    - NHFLAGS := [ onlink | pervasive ]
    - RTPROTO := [ kernel | boot | static | NUMBER ]
    - PREF := [ low | medium | high ]
    - TIME := NUMBER[s|ms]
    - BOOL := [1|0]
    - FEATURES := ecn
    - ENCAPTYPE := [ mpls | ip | ip6 | seg6 | seg6local | rpl | ioam6 | xfrm ]
    - ENCAPHDR := [ MPLSLABEL | SEG6HDR | SEG6LOCAL | IOAM6HDR | XFRMINFO ]
    - SEG6HDR := [ mode SEGMODE ] segs ADDR1,ADDRi,ADDRn [hmac HMACKEYID] [cleanup]
    - SEGMODE := [ encap | encap.red | inline | l2encap | l2encap.red ]
    - SEG6LOCAL := action ACTION [ OPTIONS ] [ count ]
    - ACTION := { End | End.X | End.T | End.DX2 | End.DX6 | End.DX4 | End.DT6 | End.DT4 | End.DT46 | End.B6 | End.B6.Encaps | End.BM | End.S | End.AS | End.AM | End.BPF }
    - OPTIONS := OPTION [ OPTIONS ]
    - OPTION := { flavors FLAVORS | srh SEG6HDR | nh4 ADDR | nh6 ADDR | iif DEV | oif DEV | table TABLEID | vrftable TABLEID | endpoint PROGNAME }
    - FLAVORS := { FLAVOR[,FLAVOR] }
    - FLAVOR := { psp | usp | usd | next-csid }
    - IOAM6HDR := trace prealloc type IOAM6_TRACE_TYPE ns IOAM6_NAMESPACE size IOAM6_TRACE_SIZE
    - XFRMINFO := if_id IF_ID [ link_dev LINK ]
    - ROUTE_GET_FLAGS := [ fibmatch ]
- Examples:
    - `ip route show` : Liệt kê tất cả các tuyến đường đang hoạt động.
    - `ip -brief route show` : Liệt kê bảng định tuyến ở dạng ngắn gọn.
    - `ip route show table main` : Liệt kê tuyến đường theo bảng định tuyến cụ thể (main).
    - `ip route get 8.8.8.8` : Liệt kê tuyến đường theo địa chỉ IP đích.
    - `ip route add 192.168.2.0/24 via 192.168.1.1` : Thêm tuyến đường đến mạng con `192.168.2.0/24` qua `gateway 192.168.1.1`.
    - `ip route add default via 192.168.1.1` : Thêm tuyến đường mặc định (default) qua `gateway 192.168.1.1`.
    - `ip route add 192.168.3.0/24 dev eth0` : Thêm tuyến đường chỉ áp dụng cho một giao diện mạng cụ thể (eth0).
    - `ip route del 192.168.2.0/24` : Xóa tuyến đường đến mạng `192.168.2.0/24`.
    - `ip route del default` : Xóa tuyến đường mặc định (default).
    - `ip route replace 192.168.2.0/24 via 192.168.1.254` : Thay thế tuyến đường đến `192.168.2.0/24` qua một gateway mới `192.168.1.254`.
    - `ip route add default nexthop via 192.168.1.1 dev eth0 nexthop via 192.168.1.2 dev eth1` : Thiết lập hai tuyến đường cân bằng tải (Load Balancing) với trọng số bằng nhau.
    - `ip route add default via 192.168.1.1 metric 100` : Thiết lập tuyến đường mặc định với metric ưu tiên thấp hơn (*ưu tiên cao hơn có số metrics nhỏ hơn*).
    - `ip route get 8.8.8.8` : Kiểm tra tuyến đường gói tin sẽ đi đến IP `8.8.8.8`.
    - `ip route get 8.8.8.8 from 192.168.1.100` : Kiểm tra tuyến đường đến địa chỉ cụ thể `8.8.8.8` từ một địa chỉ nguồn cụ thể `192.168.1.100`.

## ip rule

## ip neigh

## ip monitor

*[Lên đầu trang](#nux-root--iproute2-linux-cli)*