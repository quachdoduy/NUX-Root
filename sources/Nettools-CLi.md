# NUX-Root / NettoolsN Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](https://github.com/quachdoduy/NUX-Root/blob/main/sources/Nettools-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](https://github.com/quachdoduy/NUX-Root/blob/main/sources/Nettools-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](https://github.com/quachdoduy/NUX-Root/)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)

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

*[Back to Top](#nux-root--nettoolsn-linux-cli)*