---
title: "Upgrade Ubuntu 22.04 LTS to 24.04 LTS"
#titleLink: ""
date: 2025-08-01
description: >
  Upgrade system to Ubuntu 24.04 LTS.
categories: ["Windows", "PowerShell7"]
---

```bash
# /etc/apt/sources.list
dzamo@web-srv1-study:~$ cat /etc/apt/sources.list
deb http://us.archive.ubuntu.com/ubuntu/ jammy main restricted
deb http://us.archive.ubuntu.com/ubuntu/ jammy-updates main restricted
deb http://us.archive.ubuntu.com/ubuntu/ jammy universe
deb http://us.archive.ubuntu.com/ubuntu/ jammy-updates universe

deb http://us.archive.ubuntu.com/ubuntu/ jammy multiverse
deb http://us.archive.ubuntu.com/ubuntu/ jammy-updates multiverse
deb http://us.archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ jammy-security main restricted
deb http://archive.ubuntu.com/ubuntu/ jammy-security universe
deb http://archive.ubuntu.com/ubuntu/ jammy-security multiverse

# Update & upgrade system
dzamo@web-srv1-study:~$ sudo apt update && sudo apt -y upgrade
```
