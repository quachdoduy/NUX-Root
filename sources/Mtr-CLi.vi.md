# NUX-Root / MTR Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](Mtr-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](Mtr-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Install](#install)
    - [Refer to original document](#tham-khảo-tài-liệu-gốc)
- [COMMONLY USED COMMANDS](#commonly-used-commands)
    - [mtr](#mtr)

---

# PREFACE
- **mtr** kết hợp chức năng của các chương trình '**traceroute**' và '**ping**' trong một công cụ chẩn đoán mạng duy nhất.
- Khi mtr khởi động, nó sẽ điều tra kết nối mạng giữa máy chủ mà mtr chạy và máy chủ đích do người dùng chỉ định. Sau khi xác định địa chỉ của mỗi bước nhảy mạng giữa các máy, nó sẽ gửi một chuỗi các yêu cầu ICMP ECHO đến từng máy để xác định chất lượng liên kết đến từng máy. Khi thực hiện việc này, nó sẽ in số liệu thống kê đang chạy về từng máy.
- **mtr** được phân phối theo Giấy phép Công cộng GNU phiên bản 2.

## Install
- Với `Ubuntu 22.04` trở lên, nó đã được tích hợp theo mặc định trong trình cài đặt. Dưới đây là lệnh cài đặt cho các phiên bản không được tích hợp.
```bash
sudo apt install -y mtr
```

## Tham khảo tài liệu gốc
- webpage: http://www.BitWizard.nl/mtr/
- github: https://github.com/traviscross/mtr

---

# COMMONLY USED COMMANDS

## mtr
- Synopsis:<br>
    `mtr [options] hostname`<br>
- Parameter:
    * `-F, --filename FILE`       : read hostname(s) from a file
    * `-4`                        : use IPv4 only
    * `-6`                        : use IPv6 only
    * `-u, --udp`                 : use UDP instead of ICMP echo
    * `-T, --tcp`                 : use TCP instead of ICMP echo
    * `-I, --interface NAME`      : use named network interface
    * `-a, --address ADDRESS`     : bind the outgoing socket to ADDRESS
    * `-f, --first-ttl NUMBER`    : set what TTL to start
    * `-m, --max-ttl NUMBER`      : maximum number of hops
    * `-U, --max-unknown NUMBER`  : maximum unknown host
    * `-P, --port PORT`           : target port number for TCP, SCTP, or UDP
    * `-L, --localport LOCALPORT` : source port number for UDP
    * `-s, --psize PACKETSIZE`    : set the packet size used for probing
    * `-B, --bitpattern NUMBER`   : set bit pattern to use in payload
    * `-i, --interval SECONDS`    : ICMP echo request interval
    * `-G, --gracetime SECONDS`   : number of seconds to wait for responses
    * `-Q, --tos NUMBER`          : type of service field in IP header
    * `-e, --mpls`                : display information from ICMP extensions
    * `-Z, --timeout SECONDS`     : seconds to keep probe sockets open
    * `-M, --mark MARK`           : mark each sent packet
    * `-r, --report`              : output using report mode
    * `-w, --report-wide`         : output wide report
    * `-c, --report-cycles COUNT` : set the number of pings sent
    * `-j, --json`                : output json
    * `-x, --xml`                 : output xml
    * `-C, --csv`                 : output comma separated values
    * `-l, --raw`                 : output raw format
    * `-p, --split`               : split output
    * `-t, --curses`              : use curses terminal interface
          `--displaymode MODE`    : select initial display mode
    * `-g, --gtk`                 : use GTK+ xwindow interface
    * `-n, --no-dns`              : do not resolve host names
    * `-b, --show-ips`            : show IP numbers and host names
    * `-o, --order FIELDS`        : select output fields
    * `-y, --ipinfo NUMBER`       : select IP information in output
    * `-z, --aslookup`            : display AS number
    * `-h, --help`                : display this help and exit
    * `-v, --version`             : output version information and exit
- Examples:
    - `mtr [domainName/IP]` : Kiểm tra kết nối tới domain/IP.

*[Lên đầu trang](#nux-root--mtr-linux-cli)*