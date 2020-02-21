---
layout: post
title: SSH Agent(USB Key)验证
---

## 客户端准备SSH Agent(USB Key)公钥  

用ssh-add -L看可否列出USB Key公钥。


## 添加USB Key公钥到服务器

ssh-copy-id user@server

可测试ssh user@server不用输密码登录。



## 设置服务器/etc/ssh/sshd_config
```bash
#------------------------------------
HostKey /etc/ssh/ssh_host_ed25519_key
KexAlgorithms curve25519-sha256
AuthenticationMethods publickey
PasswordAuthentication no
#------------------------------------
```



## 设置客户端~/.ssh/known_hosts
文件每行用空白间隔的字段为markers (optional), hostnames, keytype, base64-encoded key, comment。  

如hostnames动态变化(如拨号上网)，不是域名或固定ip，可用*替代；但如该主机私钥泄露，所有其它主机会被模拟而不提示。

可用不带ip的static_hostname替换，同时ssh登录时使用HostKeyAlias及HostName参数。

```
ssh -o HostName=ip dynamic_serverhost
ssh -o HostKeyAlias="static_hostname" user@ip
```


## 设置客户端~/.ssh/config

```
Host serverhost
  User username
Host dynamic_serverhost
  User username
  HostKeyAlias "static_hostname"
```
每个server添加一段。
