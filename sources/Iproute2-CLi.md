# NUX-Root / Iproute2 Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](Iproute2-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](Iproute2-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS


# PREFACE
**iproute2** is a modern, complete replacement for **net-tools** with many improvements in performance, security and IPv6 support. If you are familiar with **net-tools**, switching to **iproute2** will help you manage your Linux network more effectively.

## Why iproute2?
- Most Linux distributions, and most UNIX's, currently use the venerable `arp`, `ifconfig` and `route` commands. While these tools work, they show some unexpected behaviour under Linux 2.2 and up. For example, GRE tunnels are an integral part of routing these days, but require completely different tools.
- With iproute2, tunnels are an integral part of the tool set.
- The 2.2 and above Linux kernels include a completely redesigned network subsystem. This new networking code brings Linux performance and a feature set with little competition in the general OS arena. In fact, the new routing, filtering, and classifying code is more featureful than the one provided by many dedicated routers and firewalls and traffic shaping products.
- As new networking concepts have been invented, people have found ways to plaster them on top of the existing framework in existing OSes. This constant layering of cruft has lead to networking code that is filled with strange behaviour, much like most human languages. In the past, Linux emulated SunOS's handling of many of these things, which was not ideal.
- This new framework makes it possible to clearly express features previously beyond Linux's reach.

## History
- In the late 1990s, **Alexey Kuznetsov**, a Linux kernel developer, created `iproute2` to replace `net-tools`. This tool was designed to work with Linux Kernel 2.2 and above, providing better support for new networking features.

| Years | Event |
|-------|------------------------------------------------------------------------------------|
| 1999  | `iproute2` was born, gradually replacing `net-tools`. Supports IPv4, advanced routing. |
| 2002  | Linux Kernel 2.4 improves many networking features, iproute2 continues to be expanded. |
| 2003-2005 | Stronger support for IPv6, **`iproute2` becomes the default tool on new Linux distributions.** |
| 2007-2010 | Linux Kernel 2.6 is released with major networking improvements, `iproute2` continues to evolve to support Netlink API. |
| 2015-2018 | `iproute2` updated to support `network namespaces`, `container networking` (*for Docker, Kubernetes*). |
| 2020 - now | Continued development, support for XDP, eBPF, Traffic Control (QoS) and new technologies in the Linux Kernel. |

## Key Technologies Supported by iproute2
1. **IPv6** – Full support, while `net-tools` has only limited support.
2. **Netlink API** – Communicates directly with the Kernel, making the ip command faster than `ifconfig`.
3. **Policy-Based Routing** – Policy based routing (ip rule), supports multiple routing tables (ip route).
4. **Traffic Control (QoS)** – Bandwidth control and queue management (`tc`).
5. **Network Namespace** – Supports container networking (Docker, Kubernetes).

## Refer to original document
- **iproute2 source code on kernel.org**: https://www.kernel.org/pub/linux/utils/net/iproute2/
- **Official Git Repository**:  https://git.kernel.org/pub/scm/network/iproute2/iproute2.git
- **iproute2 manual (man pages)**: https://man7.org/linux/man-pages/man8/
- **Linux Foundation Networking Guide**: https://wiki.linuxfoundation.org/networking/iproute2
- **Arch Linux Wiki on iproute2 (very detailed)**: https://wiki.archlinux.org/title/Iproute2
- **Debian Wiki about iproute2**: https://wiki.debian.org/IPRoute2

# COMMONLY USED COMMANDS

![Overview](../assets/images/IPRoute2.png "Overview")

## ip link
- Synopsis:<br>
    `ip link [help | show | add | delete | set]`<br>
- Description:
    - The `ip link` command in `iproute2` is used to manage network interfaces. It allows displaying information, enabling/disabling, renaming, configuring MTU, and changing the MAC address of the network card.
- Parameter:<br>
    `help` : show help.<br>
    `show` : list all network interfaces.<br>
    `add` : add network interface.<br>
    `delete` : delete network interface.<br>
    `set` : set network interface.<br>
    *For more, please refer to the original document.*
- Examples:
    - `ip link show` : View list of network interfaces.
    - `ip link show dev eth0` : List a specific interface.
    - `ip -brief link show` : Display information in a concise form.
    - `ip link set eth0 up` : Enable network interface eth0.
    - `ip link set eth0 down` : Disable the eth0 network interface.
    - `ip link set eth0 name wan0` : Rename network interface eth0 to wan0.
    - `ip link set eth0 down && ip link set eth0 up` : Restart network interface.

## ip addr
- Synopsis:<br>
    `ip addr [help | show | add | del | change | replace | flush]`<br>
    *Note: the full command is `ip address` but can be abbreviated as `ip addr`.*
- Description:
    - The `ip addr` command in `iproute2` is used to manage IP addresses on network interfaces in Linux. It can display, add, delete, and change IP addresses, as well as configure related parameters such as broadcast, netmask, and scope.
- Parameter:<br>
    `help` : show help.<br>
    `show` : list all ip network.<br>
    `add` : add ip network.<br>
    `del` : delere ip network.<br>
    *For more, please refer to the original document.*
- Examples:
    - `ip addr show` : List all IP addresses on all interfaces.
    - `ip addr show dev eth0` :  List the IP address of a specific interface (eth0).
    - `ip -brief addr show` : Displays IP address in short form.
    - `ip addr add 192.168.1.100/24 dev eth0` : Add IPv4 address to interface eth0.
    - `ip addr add 192.168.1.100/24 brd 192.168.1.255 dev eth0` : Add IP address with specific broadcast.
    - `ip addr del 192.168.1.100/24 dev eth0` : Remove IPv4 address from eth0.
    - `ip addr flush dev eth0` : Delete all IP addresses on eth0.
    - `ip addr show | grep dynamic` : View address assigned by DHCP.
    - `ip addr add 192.168.2.100/24 dev eth0 label eth0:1` : Add IP alias on eth0:1.

## ip route
- Synopsis:
    - `ip route { add | del | change | append | replace } **ROUTE**`
        * `ROUTE := NODE_SPEC [ INFO_SPEC ]`
    - `ip route { list | flush } SELECTOR`

## ip rule

## ip neigh

## ip monitor

*[Back to Top](#nux-root--iproute2-linux-cli)*