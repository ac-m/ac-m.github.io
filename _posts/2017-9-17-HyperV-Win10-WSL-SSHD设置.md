---
layout: post
title: HyperV Win10 WSL SSHD 设置
---


## 修改Win10防火墙

放行tcp进入端口22


## 生成主机密钥

```bash
sudo ssh-keygen -A
```

## 修改sshd_config

```bash
PasswordAuthentication yes
```


## 启动密码验证的sshd

```bash
sudo service ssh start
```
至此sshd启动，可以从虚拟机外层ssh进去，界面更好，继续余下步骤。


## 升级OpenSSH v7.5

按下面链接介绍升级
<https://ac-m.github.io/%E5%AE%89%E8%A3%85openssh/>


## 主机公钥签名

复制服务器主机公钥到CA主机，签名得到证书，复制服务器证书及CA公钥回服务器。
```bash
scp userA@server_host:/etc/ssh/ssh_host_ed25519_key.pub ~/ssh_ca/
ssh-keygen -s ~/ssh_ca/ca_key -I "ed25519 server host certificate" -h ~/ssh_ca/ssh_host_ed25519_key.pub
rm ~/ssh_ca/ssh_host_ed25519_key.pub
scp ~/ssh_ca/{ssh_host_ed25519_key-cert.pub,ca_key.pub} userA@server_host:
```

## sshd_config设置






