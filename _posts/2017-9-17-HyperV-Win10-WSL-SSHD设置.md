---
layout: post
title: HyperV Win10 WSL SSHD 设置
---

## HyperV Win10 WSL SSHD

在Win10上装HyperV，  
在HyperV里装Win10，  
在HyperV的Win10里装WSL，  
在HyperV的Win10的WSL里装SSHD(即OpenSSH-Server)。  


## 修改HyperV的Win10的防火墙

放行tcp进入端口22


## WSL生成主机密钥

```bash
sudo ssh-keygen -A
```

## WSL修改sshd_config

```bash
PasswordAuthentication yes
```


## WSL启动密码验证的sshd

```bash
sudo service ssh start
```
至此sshd启动，可以从虚拟机外层ssh进去，界面更好，继续余下步骤。


## WSL升级OpenSSH v7.5

按下面链接介绍升级
<https://ac-m.github.io/%E5%AE%89%E8%A3%85openssh/>


## WSL服务器主机公钥签名

复制服务器主机公钥到CA主机，签名得到证书，复制服务器证书及CA公钥回服务器。
以下在CA主机上操作。
```bash
scp userA@wsl_server_host:/etc/ssh/ssh_host_ed25519_key.pub ~/ssh_ca/
ssh-keygen -s ~/ssh_ca/ca_key -I "ed25519 server host certificate" -h ~/ssh_ca/ssh_host_ed25519_key.pub
rm ~/ssh_ca/ssh_host_ed25519_key.pub
scp ~/ssh_ca/{ssh_host_ed25519_key-cert.pub,ca_key.pub} userA@wsl_server_host:
rm ~/ssh_ca/ssh_host_ed25519_key-cert.pub
ssh userA@wsl_server_host
```

以下在WSL服务器主机上操作。
```bash
sudo cp ~/{ca_key.pub,ssh_host_ed25519_key-cert.pub} /etc/ssh/
rm ~/{ca_key.pub,ssh_host_ed25519_key-cert.pub}
```

## WSL服务器把CA公钥加入用户的授权密钥文件

```bash
mkdir -p ~/.ssh
chmod go-rwx ~/.ssh
echo "cert-authority $(cat /etc/ssh/ca_key.pub)" >>~/.ssh/authorized_keys
chmod go-rwx ~/.ssh/authorized_keys
```


## WSL服务器sshd_config设置

先修改如下值
```bash
HostKey /etc/ssh/ssh_host_ed25519_key
HostCertificate /etc/ssh/ssh_host_ed25519_key-cert.pub
HostKeyAlgorithms ssh-ed25519
KexAlgorithms curve25519-sha256@libssh.org
PubkeyAcceptedKeyTypes ssh-ed25519-cert-v01@openssh.com
```

重启sshd
```bash
sudo service ssh restart
```

从另一主机测试登录
```bash
ssh userA@wsl_server_host
```
如不需输入密码，则证书认证方式登录成功。

再修改sshd_config如下值
```bash
PasswordAuthentication no
ChallengeResponseAuthentication no
```

重启sshd，从另一主机测试登录。
