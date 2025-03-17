# NUX-Root / MTR Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](KeepAlived-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](KeepAlived-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS

---

# PREFACE
- **Keepalived** is a routing software written in C. The main goal of this project is to provide simple and robust facilities for loadbalancing and high-availability to Linux system and Linux based infrastructures. Loadbalancing framework relies on well-known and widely used Linux Virtual Server (IPVS) kernel module providing Layer4 loadbalancing. **Keepalived** implements a set of checkers to dynamically and adaptively maintain and manage loadbalanced server pool according their health. On the other hand high-availability is achieved by VRRP protocol. VRRP is a fundamental brick for router failover. In addition, **Keepalived** implements a set of hooks to the VRRP finite state machine providing low-level and high-speed protocol interactions. In order to offer fastest network failure detection, **Keepalived** implements BFD protocol. VRRP state transition can take into account BFD hint to drive fast state transition. **Keepalived** frameworks can be used independently or all together to provide resilient infrastructures.
- **Keepalived is free software**; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.

## Refer to original document


*[Back to Top](#nux-root--iproute2-linux-cli)*