# NUX-Root / MTR Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](Mtr-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](Mtr-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Install](#install)
    - [Refer to original document](#refer-to-original-document)
- [COMMONLY USED COMMANDS](#commonly-used-commands)
    - [mtr](#mtr)

---

# PREFACE
- **mtr** combines the functionality of the '**traceroute**' and '**ping**' programs in a single network diagnostic tool.
- As mtr starts, it investigates the network connection between the host mtr runs on and a user-specified destination host. After it determines the address of each network hop between the machines, it sends a sequence of ICMP ECHO requests to each one to determine the quality of the link to each machine. As it does this, it prints running statistics about each machine.
- **mtr** is distributed under the GNU General Public License version 2.

## Install
- With `Ubuntu 22.04` and above, it is already integrated by default in the installer. Below is the installation command for versions that are not integrated.
```bash
sudo apt install -y mtr
```

## Refer to original document
- webpage: http://www.BitWizard.nl/mtr/
- github: https://github.com/traviscross/mtr

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
    - `mtr [domainName/IP]` : Check connection to domain name/IP.

*[Back to Top](#nux-root--iproute2-linux-cli)*