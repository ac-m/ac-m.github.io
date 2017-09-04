---
layout: post
title: win10 WSL安装GnuPG2.2
---

## 当前系统

Win10版本1703  
Win10 OS版本15063.540  
WSL(Windows Subsystem for Linux)版本，可用sudo lsb_release -a查看，当前为Ubuntu 16.04.3 LTS

## 安装依赖
apt install build-essential


## 下载源码及签名
```bash
curl -O https://gnupg.org/ftp/gcrypt/libgpg-error/libgpg-error-1.27.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/libgpg-error/libgpg-error-1.27.tar.bz2.sig

curl -O https://gnupg.org/ftp/gcrypt/libgcrypt/libgcrypt-1.8.1.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/libgcrypt/libgcrypt-1.8.1.tar.bz2.sig
```

## 安装公钥
gpg --recv-keys 4F25E3B6 33BD3F06

## 验证签名
验证时如显示"完好的签名，来自于"，表示签名正常。  
gpg --verify libgpg-error-1.27.tar.bz2.sig
gpg --verify libgcrypt-1.8.1.tar.bz2.sig

## 安装Libgpg-error
tar -xjf libgpg-error-1.27.tar.bz2
cd libgpg-error-1.27/
./configure
make
make install
cd ..

## 安装Libgcrypt



 
