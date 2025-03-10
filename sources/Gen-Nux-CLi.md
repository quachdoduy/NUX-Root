# NUX-Root / General Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-green)](https://github.com/quachdoduy/NUX-Root/blob/main/sources/Gen-Nux-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-yellow)](https://github.com/quachdoduy/NUX-Root/blob/main/sources/Gen-Nux-CLi.vi.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS
- [Table of Contents](#nux-root--general-linux-cli)
- [General]()

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


*[Back to Top](#nux-root--general-linux-cli)*