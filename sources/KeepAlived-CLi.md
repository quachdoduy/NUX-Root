# NUX-Root / KeepAlived Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](KeepAlived-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](KeepAlived-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [PREFACE](#preface)
    - [Refer to original document](#refer-to-original-document)
    - [Refer to expanded document](#refer-to-expanded-document)
- [BASIC INSTRUCTIONS](#basic-instructions)
    - [Install](#install)
    - [Configuration](#configuration)
    - [Check Status](#check-status)
- [ADVANCED INSTRUCTIONS](#advanced-instructions)
- [COMMON EXAMPLES](#common-examples)
    - [Tracking Interface](#tracking-interface)
    - [Tracking Process](#tracking-process)
    - [Tracking Script](#tracking-script)

---

<img alt="Keep Alived" src="https://www.keepalived.org/images/ka-header-new.png">

# PREFACE
- **Keepalived** is a routing software written in C. The main goal of this project is to provide simple and robust facilities for loadbalancing and high-availability to Linux system and Linux based infrastructures. Loadbalancing framework relies on well-known and widely used Linux Virtual Server (IPVS) kernel module providing Layer4 loadbalancing. **Keepalived** implements a set of checkers to dynamically and adaptively maintain and manage loadbalanced server pool according their health. On the other hand high-availability is achieved by VRRP protocol. VRRP is a fundamental brick for router failover. In addition, **Keepalived** implements a set of hooks to the VRRP finite state machine providing low-level and high-speed protocol interactions. In order to offer fastest network failure detection, **Keepalived** implements BFD protocol. VRRP state transition can take into account BFD hint to drive fast state transition. **Keepalived** frameworks can be used independently or all together to provide resilient infrastructures.
- **Keepalived is free software**; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.

## Refer to original document
- webpage: https://www.keepalived.org/
- github: https://github.com/acassen/keepalived/
- user guide: https://keepalived.readthedocs.io
- social: https://x.com/keepalived

## Refer to expanded document
- RFC 9568: https://datatracker.ietf.org/doc/html/rfc9568

---

# BASIC INSTRUCTIONS

## Install
- Install keepalived (for all nodes in cluster).
```
sudo apt install -y keepalived
```
- Modify keepalived.conf file.
```ssh
sudo nano /etc/keepalived/keepalived.conf
```

## Configuration
- The short form is as follows.
```bash
vrrp_instance string {      # identify a VRRP instance definition block
    state MASTER|BACKUP     # specify the instance state in standard use
    interface string        # specify the network interface for the instance to run on
    virtual_router_id num   # specify to which VRRP router id the instance belongs
    priority num            # specify the instance priority in the VRRP router (range from 1 to 255)
    advert_int num          # specify the advertisement interval in seconds (set to 1)
    authentication {        # identify a VRRP authentication definition block
        auth_type PASS|AH   # specify which kind of authentication to use (PASS|AH)
        auth_pass string    # specify the password string to use
    }
    virtual_ipaddress {     # identify a VRRP VIP definition block (Block limited to 20 IP addresses) 
        @IP
        @IP
        @IP
    }
}
```
- Example.
```bash
# *Node Master*
vrrp_instance VipKA {
    state MASTER            # state of note is MASTER.
    interface ens33         # vrrp_instance network interface is ens33.
    virtual_router_id 69    # vr_id is 69
    priority 100            # priority is 100
    advert_int 1            # advertisement interval is 1s.
    authentication {
        auth_type PASS
        auth_pass V3ryS3cr3t
    }
    virtual_ipaddress {
        192.168.69.1        # VIP is 192.168.69.1
    }
}
```
```bash
# *Node Backup*
vrrp_instance VipKA {
    state BACKUP            # state of note is BACKUP.
    interface ens33         # vrrp_instance network interface is ens33.
    virtual_router_id 69    # vr_id is 69
    priority 99             # priority is 99
    advert_int 1            # advertisement interval is 1s.
    authentication {
        auth_type PASS
        auth_pass V3ryS3cr3t
    }
    virtual_ipaddress {
        192.168.69.1        # VIP is 192.168.69.1
    }
}
```
-  Restart keepalived service.
```
sudo systemctl restart keepalived
```

## Check Status
- Check which node is the Master.
```
ip addr show | grep "Virtual_IP"
```
- View Keepalived VRRP status.
```
watch -n 1 "ip -br addr show dev your_NIC_nae"
```
- View Keepalived log.
```
journalctl -u keepalived --no-pager | tail -50
```

---

# ADVANCED INSTRUCTIONS
- The Master election process is divided into 3 main stages, following the latest standards currently issued for VRRP version 3.
    - Initialize
    - Active
    - Backup

<img alt="Master Election" src="../assets/images/Master Voting Process.png" />

- A VRRP virtual router uses the **MAC address 00:00:5E:00:01:XX**, being XX the Virtual Router Identifier (VRID), which is different for each virtual router in the network. Physical routers within the virtual router (that means, each instance spawned in a host which is part of a virtual router) must communicate within themselves using packets with **multicast address version 4 224.0.0.18 (v6 FF02::12)** and **IP protocol 112**. In a nutshell, this is what you’ll see capturing in the HA interface:

<img alt="VRRP Cap" src="../assets/images/VRRP_cap.png" />

---

# COMMON EXAMPLES
*Notes: The examples below are only brief on 1 node.*

## Tracking Interface
To monitor a network interface, you use the `track_interface` block in the `vrrp_instance` configuration. If the monitored interface fails, the priority of `vrrp_instance` is reduced, leading to a MASTER switch if necessary.

```bash
# *Node Master*
vrrp_instance VipKA {
    state MASTER            # state of note is MASTER.
    interface ens33         # vrrp_instance network interface is ens33.
    virtual_router_id 69    # vr_id is 69
    priority 100            # priority is 100
    advert_int 1            # advertisement interval is 1s.
    authentication {
        auth_type PASS
        auth_pass V3ryS3cr3t
    }
    virtual_ipaddress {
        192.168.69.1        # VIP is 192.168.69.1
    }
    track_interface {       # command block used to monitoring interface.
        eth01               # the interface that to monitoring.
    }
}
```
*In this example, in addition to monitoring `ens33`, Keepalived also monitors `eth01`. If `eth01` fails, the ***priority*** of `vrrp_instance` will decrease.*

## Tracking Process
To monitor a process/service, you use the `vrrp_track_process` block and reference it in `vrrp_instance`. If the monitored process stops running, its priority is reduced by the specified value.

```bash
# *Node Master*
vrrp_track_process chk_httpd {  # command block used to check process/service.
    process "httpd"             # the process/service that to check.
    weight -10                  # the priority index will decrease when process/service is not running.
}
vrrp_instance VipKA {
    state MASTER            # state of note is MASTER.
    interface ens33         # vrrp_instance network interface is ens33.
    virtual_router_id 69    # vr_id is 69
    priority 100            # priority is 100
    advert_int 1            # advertisement interval is 1s.
    authentication {
        auth_type PASS
        auth_pass V3ryS3cr3t
    }
    virtual_ipaddress {
        192.168.69.1        # VIP is 192.168.69.1
    }
    track_process {         # command block used to monitoring process/service
        chk_httpd           # the name of code block used to check process/service.
    }
}
```
*In this example, Keepalived monitors the `httpd` process. If `httpd` stops, the ***priority*** of `vrrp_instance` will decrease by 10.*

## Tracking Script
You can use custom scripts to check the health of a system or service. The `vrrp_script` block defines the script to run and the weight that affects the priority based on the script's results.

```bash
# *Node Master*
vrrp_script chk_nginx {                     # command block used to check script.
    script "/usr/local/bin/check_nginx.sh"  # the script that to check nginx status.
    interval 5                              # the interval time to run script.
    weight -20                              # the priority index will decrease when script return false.
}
vrrp_instance VipKA {
    state MASTER            # state of note is MASTER.
    interface ens33         # vrrp_instance network interface is ens33.
    virtual_router_id 69    # vr_id is 69
    priority 100            # priority is 100
    advert_int 1            # advertisement interval is 1s.
    authentication {
        auth_type PASS
        auth_pass V3ryS3cr3t
    }
    virtual_ipaddress {
        192.168.69.1        # VIP is 192.168.69.1
    }
    track_script {          # command block used to monitoring script.
        chk_nginx           # the name of code block used to check script.
    }
}
```
*In this example, the `check_nginx.sh` script will run every 5 seconds to check the status of `Nginx`. ***If the script returns fail***, the ***priority*** of `vrrp_instance` will be decreased by 20.*

*[Back to Top](#nux-root--keepalived-linux-cli)*