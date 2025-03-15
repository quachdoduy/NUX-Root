# NUX-Root / Nettools Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](Nettools-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](Nettools-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Inception and initial development](#inception-and-initial-development)
    - [Versions and major developments](#versions-and-major-developments)
    - [Stop development and replace](#stop-development-and-replace)
    - [Modern replacement tools](#modern-replacement-tools)
    - [Current status](#current-status)
    - [Refer to original document](#refer-to-original-document)
- [COMMONLY USED COMMANDS](#commonly-used-commands)
    - [ifconfig](#ifconfig)
    - [arp](#arp)
    - [route](#route)
    - [netstat](#netstat)

---

# PREFACE
The **net-tools** toolkit is a collection of network management commands on Unix/Linux operating systems, which was widely used for many years before being replaced by **iproute2**.

## Inception and initial development
- Origin: **net-tools** appeared in the early days of Linux, around the 1990s.
- Purpose: Provides basic tools for network management, including network interface configuration (*ifconfig*), connectivity testing (*netstat*), routing table management (*route*), and other commands.
- Main author: **Bernd Eckenfels** was the main maintainer in the later stages, but many other developers have also contributed.

## Versions and major developments
- Last stable version: `net-tools 1.60`, released in 2001.
- Last update: Several minor updates have since appeared on Linux distributions, but no new official versions.

## Stop development and replace
- Because the **net-tools** suite was old and did not support modern networking features such as *IPv6*, *policy routing*, and *network namespaces* well, it was replaced by the **iproute2** suite in the late 2000s.
- Linux distributions like `Debian`, `Ubuntu`, and `RHEL` have gradually **removed net-tools** from the default installation.

## Modern replacement tools
- The **iproute2** suite was developed to replace **net-tools**, with new commands:
    - `ip addr` replace `ifconfig`
    - `ip route` replace `route`
    - `ss` replace `netstat`
    - `ip neighbor` replace `arp`

## Current status
- Although no longer actively maintained, **net-tools** is still present on some legacy systems or lightweight Linux distributions.
- Some forks exist on GitHub, but are not officially updated.
- Modern users are recommended to use **iproute2** instead of **net-tools**.

## Refer to original document
- SourceForge (net-tools root archive): https://sourceforge.net/projects/net-tools/
- Bernd Eckenfels's GitHub: https://github.com/ecki/net-tools
- Linux Kernel Archives page: https://www.kernel.org/

---

# COMMONLY USED COMMANDS

## ifconfig
- Synopsis:<br>
    `ifconfig [-v] [-a] [-s] [interface]`<br>
    `ifconfig [-v] interface [aftype] options | address ...`
- Description:
    - Ifconfig is used to configure the kernel-resident network interfaces. It is used at boot time to set up interfaces as necessary. After that, it is usually only needed when debugging or when system tuning is needed.
    - If no arguments are given, ifconfig displays the status of the currently active interfaces. If a single interface argument is given, it displays the status of the given interface only; if a single -a argument is given, it displays the status of all interfaces, even those that are down. Otherwise, it configures an interface.
- Options:<br>
    `-a` : display all interfaces which are currently available, even if down.<br>
    `-s` : display a short list (like `netstat -i`).<br>
    `-v` : be more verbose for some error conditions.<br>
    *For more, please refer to the original document.*
- Examples:<br>
    `ifconfig` : Displays all active network interfaces (with IP addresses).<br>
    `ifconfig -a` : Show all network interfaces including inactive interfaces.<br>
    `ifconfig eth0 192.168.1.100 netmask 255.255.255.0` : Assign IP address `192.168.1.100` with subnet mask `255.255.255.0` to interface `eth0`.<br>
    `ifconfig eth0 up` : Enable network interface.<br>
    `ifconfig eth0 down` : Disable network interface.

## arp
- Synopsis:<br>
    `arp [-vn] [-H type] [-i if] [-ae] [hostname]`<br>
    `arp [-v] [-i if] -d hostname [pub]`<br>
    `arp [-v] [-H type] [-i if] -s hostname hw_addr [temp]`<br>
    `arp [-v] [-H type] [-i if] -s hostname hw_addr [netmask nm] pub`<br>
    `arp [-v] [-H type] [-i if] -Ds hostname ifname [netmask nm] pub`<br>
    `arp [-vnD] [-H type] [-i if] -f [filename]`
- Description:
    - Arp manipulates or displays the kernel's IPv4 network neighbour cache. It can add entries to the table, delete one or display the current content.
    - ARP stands for Address Resolution Protocol, which is used to find the media access control address of a network neighbour for a given IPv4 Address.
- Modes:
    - **arp** with no mode specifier will print the current content of the table. It is possible to limit the number of entries printed, by specifying an hardware address type, interface name or host address.
    - **arp -d** *address* will delete a ARP table entry. Root or netadmin privilege is required to do this. The entry is found by IP address. If a hostname is given, it will be resolved before looking up the entry in the ARP table.
    - **arp -s** *address hw_addr* is used to set up a new table entry. The format of the hw_addr parameter is dependent on the hardware class, but for most classes one can assume that the usual presentation can be used. For the Ethernet class, this is 6 bytes in hexadecimal, separated by colons. When adding proxy arp entries (that is those with the publish flag set) a netmask may be specified to proxy arp for entire subnets. This is not good practice, but is supported by older kernels because it can be useful. If the temp flag is not supplied entries will be permanent stored into the ARP cache. To simplify setting up entries for one of your own network interfaces, you can use the arp -Ds address ifname form. In that case the hardware address is taken from the interface with the specified name.
- Options:<br>
    `-v` or `--verbose` : Tell the user what is going on by being verbose.<br>
    `-n` or `--numeric` : Shows numerical addresses instead of trying to determine symbolic host, port or user names.<br>
    `-D` or `--use-device` : Instead of a hw_addr, the given argument is the name of an interface. arp will use the MAC address of that interface for the table entry. This is usually the best option to set up a proxy ARP entry to yourself.<br>
    *For more, please refer to the original document.*
- Examples:<br>
    `arp -a` : Displays all entries in the ARP table, including IP address, MAC address, and status.<br>
    `arp -n` : Show ARP table without host name resolution, only show IP and MAC addresses.<br>
    `arp -d 192.168.1.1` : Remove IP address 192.168.1.1 from the ARP table.<br>
    `arp -i eth0 -Ds 10.0.0.2 eth1 pub` : This will answer ARP requests for 10.0.0.2 on eth0 with the MAC address for eth1.<br>
    `/usr/sbin/arp -i eth1 -d 10.0.0.1` : Delete the ARP table entry for 10.0.0.1 on interface eth1. This will match published proxy ARP entries and permanent entries.

## route
- Synopsis:<br>
    `route [-CFvnNee] [-A family |-4|-6]`
- Description:
    - **Route** manipulates the kernel's IP routing tables. Its primary use is to set up static routes to specific hosts or networks via an interface after it has been configured with the [ifconfig](#ifconfig)(8) program.
    - When the **add** or **del** options are used, **route** modifies the routing tables. Without these options, **route** displays the current contents of the routing tables.
- Options:<br>
    `-A family` : use the specified address family (eg `inet`). Use **route --help** for a full list. You can use **-6** as an alias for **--inet6** and **-4** as an alias for **-A inet**.<br>
    `-F` : operate on the kernel's FIB (Forwarding Information Base) routing table. This is the default.<br>
    `-C` : operate on the kernel's routing cache.<br>
    `-n` : show numerical addresses instead of trying to determine symbolic host names. This is useful if you are trying to determine why the route to your nameserver has vanished.<br>
    `del` : delete a route.<br>
    `add` : add a new route.<br>
    *For more, please refer to the original document.*
- Examples:<br>
    `route add -net 127.0.0.0 netmask 255.0.0.0 dev lo` : adds the normal **loopback** entry, using netmask **255.0.0.0** and associated with the "**lo**" device.<br>
    `route add -net 192.56.76.0 netmask 255.255.255.0 dev eth0` : adds a route to the local network **192.56.76.x** via "**eth0**". The word "**dev**" can be omitted here.<br>
    `route add -net 224.0.0.0 netmask 240.0.0.0 dev eth0` : This is an obscure one documented so people know how to do it. This sets all of the **class D (multicast)** IP routes to go via "**eth0**". This is the correct normal configuration line with a multicasting kernel.

## netstat
- Notes:
    *This program is obsolete.*
    - Replacement for **netstat** is **ss**.
    - Replacement for **netstat -r** is **ip route**.
    - Replacement for **netstat -i** is **ip -s link**.
    - Replacement for **netstat -g** is **ip maddr**.
- Synopsis:<br>
    `netstat [*address_family_options*] [--tcp|-t] [--udp|-u] [--udplite|-U] [--raw|-w] [--listening|-l] [--all|-a] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--symbolic|-N] [--extend|-e[--extend|-e]] [--timers|-o] [--program|-p] [--verbose|-v] [--continuous|-c] [--wide|-W] netstat {--route|-r} [address_family_options] [--extend|-e[--extend|-e]] [--verbose|-v] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--interfaces|-i} [--all|-a] [--extend|-e[--extend|-e]] [--verbose|-v] [--program|-p] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--groups|-g} [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--masquerade|-M} [--extend|-e] [--numeric|-n] [--numeric-hosts] [--numeric-ports] [--numeric-users] [--continuous|-c] netstat {--statistics|-s} [--tcp|-t] [--udp|-u] [--udplite|-U] [--raw|-w] netstat {--version|-V} netstat {--help|-h} *address_family_options*:`<br>
    `[-4|--inet] [-6|--inet6] [--protocol={inet,inet6,unix,ipx,ax25,netrom,ddp, ... } ] [--unix|-x] [--inet|--ip|--tcpip] [--ax25] [--x25] [--rose] [--ash] [--ipx] [--netrom] [--ddp|--appletalk] [--econet|--ec]`
- Description:
    - **Netstat** prints information about the Linux networking subsystem.
    - The type of information printed is controlled by the first argument, as follows:<br>
        `-r` or `--route` : Display the kernel routing tables.<br>
        `-g` or `--groups` : Display multicast group membership information for IPv4 and IPv6.<br>
        `-i` or `--interfaces` : Display a table of all network interfaces.<br>
        `-M` or `--masquerade` : Display a list of masqueraded connections.<br>
        `-s` or `--statistics` : Display summary statistics for each protocol.
- Options:<br>
    `-v` or `--verbose` : Tell the user what is going on by being verbose. Especially print some useful information about unconfigured address families.<br>
    `-W` or `--wide` : Do not truncate IP addresses by using output as wide as needed. This is optional for now to not break existing scripts.<br>
    `-n` or `--numeric` : Show numerical addresses instead of trying to determine symbolic host, port or user names.<br>
    *For more, please refer to the original document.*
- Examples:<br>
    `netstat -a` : Displays all active sockets (TCP/UDP), including listening ports (`LISTEN`).<br>
    `netstat -l` : Displays a list of ports that the system is listening on.<br>
    `netstat -an | grep ":22"` : Check connections on port `22` (SSH).<br>
    `netstat -utnpl` : My favorite command.

*[Back to Top](#nux-root--nettools-linux-cli)*