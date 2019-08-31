---
layout: post
title: SSH Agent(USB Key)验证
---

## 生成服务器主机证书
```bash
#把SSH Agent(USB Key)公钥复制到服务器agent_key.pub
#如在Windows上使用USBKey，设置Klopatra，打勾“设置|配置|GnuPG系统|Private Keys|Do not use the PIN cache when signing”。
#用Forward Agent方式登录Server
ssh -A user@server
sudo ssh-keygen -t ed25519 -N "" -C "ed25519 server host key" -f /etc/ssh/ssh_host_ed25519_key
ssh-keygen -Us agent_key.pub -I "servername ed25519 host certificate" -h /etc/ssh/ssh_host_ed25519_key.pub -V "+52w1d"
#得到ssh_host_ed25519_key-cert.pub
```
第5行，生成私钥/etc/ssh/ssh_host_ed25519_key、公钥/etc/ssh/ssh_host_ed25519_key.pub  

第6行，用客户端的SSH Agent(USB Key)私钥签名公钥ssh_host_ed25519_key.pub得到证书ssh_host_ed25519_key-cert.pub，  

其中-h表示这是主机证书而非用户证书，  

其中-V "+52w1d"表示有效期为52周(week)1天(day)，也可用日期表示，如"20190901"、具体到秒"20190901033400"，  

也可用-n principals指定主机名，客户端用ssh -o HostKeyAlias=principal方式验证服务器。  



## 设置服务器/etc/ssh/sshd_config
```bash
HostKey /etc/ssh/ssh_host_ed25519_key
HostCertificate /etc/ssh/ssh_host_ed25519_key-cert.pub

#server for client, use certificate mode
HostKeyAlgorithms ssh-ed25519-cert-v01@openssh.com

KexAlgorithms curve25519-sha256

#client for server, use pubkey not certificate
#PubkeyAcceptedKeyTypes ssh-ed25519-cert-v01@openssh.com
PubkeyAcceptedKeyTypes ssh-ed25519

AuthenticationMethods publickey
PasswordAuthentication no
ChallengeResponseAuthentication no
```

## 把SSH Agent(USB Key)公钥加入用户的授权密钥文件
准备~/.ssh/authorized_keys文件
```bash
mkdir -p ~/.ssh
chmod go-rwx ~/.ssh
touch ~/.ssh/authorized_keys
chmod go-rwx ~/.ssh/authorized_keys
cat agent_key.pub >>~/.ssh/authorized_keys
```

authorized_keys文件每行格式为空格分割的字段：options(可选) keytype base64-key comment  

keytype为ssh-ed25519;  

options字段是以','分割的option，可以为以下：  

cert-authority 表示base64-key为待认证客户端证书的允许CA公钥。  

principals="user1,user2" 表示待认证客户端证书中允许的principals。即principals之一必须出现在证书principals中，即principals与证书principals交集必须非空，认证才通过。这里user1、user2是虚拟用户，可以不是server上用户。  

from="pattern1,pattern2,pattern3" pattern可以是含'*'、'?'组成的ip地址、域名，限定IP、域名访问。  

例子，
```bash
cert-authority,principals="user1,user2",from="*.abc.com,192.168.1.?,192.168.2.*,192.168.3.1" ssh-ed25519 base64-key this is comment
cert-authority,principals=user1 ssh-ed25519 base64-key this is comment
```
```bash
echo "cert-authority,principals=user1 $(cat ca_key.pub)" >>~/.ssh/authorized_keys
```

## 设置客户端~/.ssh/known_hosts
文件每行用空白间隔的字段为markers (optional), hostnames, keytype, base64-encoded key, comment。  

验证服务器证书时marker为“@cert-authority”，hostname为"*"匹配所有主机。  

当客户端用ssh -o HostKeyAlias=principal方式验证服务器时，principal即为要在known_hosts文件中查找的hostname字段。  

```
echo "cert-authority * $(cat agent_key.pub)" >>~/.ssh/known_hosts
```
