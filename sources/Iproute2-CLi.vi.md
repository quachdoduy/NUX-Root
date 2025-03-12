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
| 2020 - now | Tiếp tục phát triển, hỗ trợ XDP, eBPF, Traffic Control (QoS) và các công nghệ mới trong Linux Kernel. |

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

## ip link

## ip addr

## ip route

## ip rule

## ip neigh

## ip monitor

*[Lên đầu trang](#nux-root--iproute2-linux-cli)*