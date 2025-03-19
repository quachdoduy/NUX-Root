# NUX-Root / KeepAlived Hierachy
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](KeepAlived-Hierachy.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](KeepAlived-Hierachy.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS

---

# TOP HIERACHY
Keepalived configuration file is articulated around a set of configuration blocks. Each block is focusing and targetting a specific daemon family feature. These features are:
- [GLOBAL CONFIGURATION](#global-configuration)
- [BFD CONFIGURATION](#bfd-configuration)
- [VRRPD CONFIGURATION](#vrrpd-configuration)
- [LVS CONFIGURATION](#lvs-configuration)

---

# GLOBAL CONFIGURATION
contains subblocks of:
- Global definitions
- Linkbeat interfaces
- Interface up/down transition delays
- Static track groups
- Static addresses
- Static routes
- Static rules

# BFD CONFIGURATION
This is an implementation of RFC5880 (Bidirectional forwarding detection), and this can be configured to work between 2 keepalived instances, but using unweighted track_bfds between a master/backup pair of VRRP instances means that the VRRP instance will only be able to come up if both VRRP instance are running, which somewhat defeats the purpose of VRRP.

This implementation has been tested with OpenBFDD (available at https://github.com/dyninc/OpenBFDD).
- BFD Instance

# VRRPD CONFIGURATION
contains subblocks of: 
- VRRP script(s)
- VRRP synchronization group(s)
- VRRP gratuitous ARP and unsolicited neighbour advert delay group(s)
- VRRP instance(s)

# LVS CONFIGURATION
contains subblocks of:
- Virtual server group(s)
- Virtual server(s)

The subblocks contain arguments for configuring Linux IPVS (LVS) feature. Knowledge of ipvsadm(8) will be helpful here. Configuring LVS is achieved by defining virtual server groups, virtual servers and optionally SSL configuration. Every virtual server defines a set of real servers, you can attach healthcheckers to each real server. Keepalived will then lead LVS operation by dynamically maintaining topology.

For details of what configuration combinations are valid, see the ipvsadm(8) man page.

**Note**: Where an option can be configured for a virtual server, real server, and possibly checker, the virtual server setting is the default for real servers, and the real server setting is the default for checkers.

**Note**: Tunnelled real/sorry servers can differ from the address family of the virtual server and non tunnelled real/sorry servers, which all have to be the same. If a virtual server uses a fwmark, and all the real/sorry servers are tunnelled, the address family of the virtual server will be the same as the address family of the real/sorry servers if they are all the same, otherwise it will default to IPv4 (use ip_family inet6 to override this).

**Note**: The port for the virtual server can only be omitted if the virtual service is persistent.

---

# SUBBLOCKS OF - GLOBAL CONFIGURATION

## Global definitions
>Following are global daemon facilities for running keepalived in a separate network namespace:<br>Set the network namespace to run in. The directory /run/keepalived will be created as an unshared mount point, for example for pid files. syslog entries will have _NAME appended to the ident.<br>*Note: the namespace cannot be changed on a configuration reload.*
```
net_namespace NAME
```

>Add the IPVS configuration in the specified net namespace. It allows to easily split the VIP traffic on a given namespace and keep the healthchecks traffic in another namespace. If NAME is not specified, then the default namespace will be used.
```
net_namespace_ipvs NAME
```

>ipsets wasn't network namespace aware until Linux 3.13, and so if running with an earlier version of the kernel, by default use of ipsets is disabled if using a namespace and vrrp_ipsets has not been specified. This options overrides the default and allows ipsets to be used with a namespace on kernels prior to 3.13.
```
namespace_with_ipsets
```

>If multiple instances of keepalived are run in the same namespace, this will create pid files with NAME as part of the file names, in /run/keepalived.<br>*Note: the instance name cannot be changed on a configuration reload.*
```
instance NAME
```

>Create pid files in /run/keepalived
```
use_pid_dir
```

>Poll to detect media link failure using ETHTOOL, MII or ioctl interface otherwise uses netlink interface.
```
linkbeat_use_polling
```

>Time for main process to allow for child processes to exit on termination in seconds. This can be needed for very large configurations.<br>*(default: 5)*
```
child_wait_time SECS
```

*Note: All processes/scripts run by keepalived are run with parent death signal set to SIGTERM. All such processes/scripts should either not change the action for SIGTERM, or ensure that the process/script terminates once SIGTERM is received, possibly following any cleanup actions needed.*

### Global definitions configuration block

global_defs {
    [tmp_config_directory DIRECTORY]( "In order to ensure that all processes read exactly the same configuration, while the config is first read it is written, by default, to a memory based file (or to an anonymous file in /tmp/ if memfd_create() is not supported).<br> If your configuration is very large, you may not want the copy to be held in memory, in which case specifing the tmp_config_directory causes the configuration to be written to an anonymous file on the filesystem on which the specified directory resides, which must be writeable by keepalived.<br> This setting cannot be changed on a reload, and it should be specified as early as possible in the configuration.")
    tmp_config_directory DIRECTORY
}


*[Back to Top](#nux-root--keepalived-hierachy)*