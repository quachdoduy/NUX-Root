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

## Why iproute2 ?
- Most Linux distributions, and most UNIX's, currently use the venerable `arp`, `ifconfig` and `route` commands. While these tools work, they show some unexpected behaviour under Linux 2.2 and up. For example, GRE tunnels are an integral part of routing these days, but require completely different tools.
- With iproute2, tunnels are an integral part of the tool set.
- The 2.2 and above Linux kernels include a completely redesigned network subsystem. This new networking code brings Linux performance and a feature set with little competition in the general OS arena. In fact, the new routing, filtering, and classifying code is more featureful than the one provided by many dedicated routers and firewalls and traffic shaping products.
- As new networking concepts have been invented, people have found ways to plaster them on top of the existing framework in existing OSes. This constant layering of cruft has lead to networking code that is filled with strange behaviour, much like most human languages. In the past, Linux emulated SunOS's handling of many of these things, which was not ideal.
- This new framework makes it possible to clearly express features previously beyond Linux's reach.

## History
- In the late 1990s, Alexey Kuznetsov, a Linux kernel developer, created iproute2 to replace net-tools. This tool was designed to work with Linux Kernel 2.2 and above, providing better support for new networking features.

| Years | Event |
|-------|------------------------------------------------------------------------------------|
| 1999  | iproute2 was born, gradually replacing net-tools. Supports IPv4, advanced routing. |
| 2002  | Linux Kernel 2.4 improves many networking features, iproute2 continues to be expanded. |
| 2003-2005 | Stronger support for IPv6, iproute2 becomes the default tool on new Linux distributions. |
| 2007-2010 | Linux Kernel 2.6 is released with major networking improvements, iproute2 continues to evolve to support Netlink API. |
| 2015-2018 | iproute2 updated to support network namespaces, container networking (for Docker, Kubernetes). |
| 2020 - now | Continued development, support for XDP, eBPF, Traffic Control (QoS) and new technologies in the Linux Kernel. |

*[Back to Top](#nux-root--iproute2-linux-cli)*