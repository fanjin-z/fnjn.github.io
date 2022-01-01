layout: post
title:  "SSH Sever Authentication Config for Root and users"
date:   2018-11-29 00:00:00 -0800
categories: SSH, Linxu
---

Here to achieve:
1. Root login with key authentication only
2. Non-root users login with key or password authentication

Edit `/etc/ssh/sshd_config`
```
PermitRootLogin without-password
PasswordAuthentication yes
```

Restart ssh service
```
systemctl restart sshd
```
