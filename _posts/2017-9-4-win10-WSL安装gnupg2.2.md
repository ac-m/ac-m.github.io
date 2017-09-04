---
layout: post
title: win10 WSL安装GnuPG2.2
---

## 当前系统

Win10版本1703  
Win10 OS版本15063.540  
WSL(Windows Subsystem for Linux)版本，可用sudo lsb_release -a查看，当前为Ubuntu 16.04.3 LTS

## 安装依赖
```bash
apt install libgnutls-dev bzip2 make gettext texinfo gnutls-bin build-essential libbz2-dev zlib1g-dev libncurses5-dev libsqlite3-dev libldap2-dev
```

## 新建临时目录
```bash
mkdir gnugp2.2 && cd gnugp2.2
```

## 下载源码及签名
```bash
curl -O https://gnupg.org/ftp/gcrypt/libgpg-error/libgpg-error-1.27.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/libgpg-error/libgpg-error-1.27.tar.bz2.sig

curl -O https://gnupg.org/ftp/gcrypt/libgcrypt/libgcrypt-1.8.1.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/libgcrypt/libgcrypt-1.8.1.tar.bz2.sig

curl -O https://gnupg.org/ftp/gcrypt/libksba/libksba-1.3.5.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/libksba/libksba-1.3.5.tar.bz2.sig

curl -O https://gnupg.org/ftp/gcrypt/libassuan/libassuan-2.4.3.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/libassuan/libassuan-2.4.3.tar.bz2.sig

curl -O https://gnupg.org/ftp/gcrypt/npth/npth-1.5.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/npth/npth-1.5.tar.bz2.sig

curl -O https://gnupg.org/ftp/gcrypt/pinentry/pinentry-1.0.0.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/pinentry/pinentry-1.0.0.tar.bz2.sig

curl -O https://gnupg.org/ftp/gcrypt/gnupg/gnupg-2.2.0.tar.bz2
curl -O https://gnupg.org/ftp/gcrypt/gnupg/gnupg-2.2.0.tar.bz2.sig
```

## 安装公钥
```bash
gpg --list-keys
gpg --recv-keys 4F25E3B6 33BD3F06
```

## 验证签名
验证时如显示"完好的签名，来自于"，表示签名正常。  
```bash
gpg --verify npth-1.5.tar.bz2.sig
gpg --verify libgpg-error-1.27.tar.bz2.sig
gpg --verify libgcrypt-1.8.1.tar.bz2.sig
gpg --verify libksba-1.3.5.tar.bz2.sig
gpg --verify libassuan-2.4.3.tar.bz2.sig
gpg --verify pinentry-1.0.0.tar.bz2.sig
gpg --verify gnupg-2.2.0.tar.bz2.sig
```

## 设置lib路径
```bash
echo "/usr/local/lib" > /etc/ld.so.conf.d/gpg2.2.conf
```

## 安装nPth
```bash
tar -xjf npth-1.5.tar.bz2
cd npth-1.5/
./configure && make && make install
cd .. && ldconfig
```

## 安装Libgpg-error
```bash
tar -xjf libgpg-error-1.27.tar.bz2
cd libgpg-error-1.27/
./configure && make && make install
cd .. && ldconfig
```

## 安装Libgcrypt
```bash
tar -xjf libgcrypt-1.8.1.tar.bz2
cd libgcrypt-1.8.1/
./configure && make
ldconfig -v
make check
make install
cd .. && ldconfig
```

## 安装Libksba
```bash
tar -xjf libksba-1.3.5.tar.bz2
cd libksba-1.3.5/
./configure && make && make install
cd .. && ldconfig
```

## 安装Libassuan
```bash
tar -xjf libassuan-2.4.3.tar.bz2
cd libassuan-2.4.3/
./configure && make && make install
cd .. && ldconfig
```

## 安装Pinentry
```bash
tar -xjf pinentry-1.0.0.tar.bz2
cd pinentry-1.0.0/
./configure --enable-pinentry-curses
make && make install
cd .. && ldconfig
```

## 安装GnuPG
```bash
tar -xjf gnupg-2.2.0.tar.bz2
cd gnupg-2.2.0/
./configure
make
make check
make install
cd .. && ldconfig -v
```

