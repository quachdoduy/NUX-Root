# NUX-Root / General Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](Gen-Nux-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](Gen-Nux-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [ENABLE ROOT USER](#enable-root-user)
- [ALLOW SSH FOR ROOT USER](#allow-ssh-for-root-user)
- [REMOVE USER ACCOUNT](#remove-user-account)
- [INSTALL TOOLS](#install-tools)
- [UPDATE NEW PACKAGE](#update-new-package)

`---`

# ENABLE ROOT USER
>By default, Ubuntu server system does not enable root account after successful installation. Therefore, we enable root account as below.

- Set a password for the `root` account.
```bash
sudo passwd root
```
*Then enter the new password for the root account.*
- Switch to `root` account.
```bash
sudo -
```

# ALLOW SSH FOR ROOT USER
>By default, the root account after being enabled still cannot access the Ubuntu server using SSH method. Therefore, we enable SSH access for the root account as follows.

- Edit file `sshd_config`. *(SSH config file)*
```bash
sudo nano /etc/ssh/sshd_config
```
- All edited lines need to remove comment (**#**) at the beginning of the line removed.
- At the line **#PermitRootLogin prohibit-password**, change it to **PermitRootLogin yes**.
- At the line **#PasswordAuthentication yes**, change it to **PasswordAuthentication yes**.
- Save change by hotkey `CTRL + O`. *(over-write)*
- Exit nano by hotkey `CTRL + X`. *(exit)*
- Restart SSH Service.
```bash
sudo systemctl restart ssh
```

- Test SSH connection with root account.
```bash
sudo ssh root@<"ip_server">
```

# REMOVE USER ACCOUNT
>By default, the Ubuntu server system has initialized an administrator account (not root) that you entered during the installation process. Therefore, we delete the administrator account as below.
*Note: do this after successfully activating the root account.*

- Double check the `sudo` rights of the account to be deleted.
```bash
sudo cat /etc/sudoers.d/<"account_to_be_deleted">
```
*If that account has sudo rights you should delete this file first.*
```bash
sudo rm /etc/sudoers.d/<"account_to_be_deleted">
```

- Check user list before deleting.
```bash
cat /etc/passwd | grep <"account_to_be_deleted">
```
- Perform administrator account deletion.
```bash
userdel -r <"account_to_be_deleted">
```

# INSTALL TOOLS
>There are some essential tools for administrators (in my opinion) that are not installed by default on Ubuntu servers.

List Tools:
- [Net-tools](https://sourceforge.net/projects/net-tools/)
- Telnet Client
- [Tracerout](https://sourceforge.net/projects/traceroute/)
```bash
sudo apt install -y net-tools telnet traceroute
```

# UPDATE NEW PACKAGE
>Always perform the latest update before installing the service.
```bash
sudo apt update && sudo apt upgrade -y
```

*[Back to Top](#nux-root--general-linux-cli)*