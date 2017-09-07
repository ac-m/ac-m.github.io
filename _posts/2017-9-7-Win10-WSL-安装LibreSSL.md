---
layout: post
title: Win10 WSL 安装LibreSSL
---

## 为何使用LibreSSL

LibreSSL是OpenSSL的后继者，OpenSSL历年漏洞比LibreSSL多。

## 下载、校验签名

略

## 编译安装

```bash
./configure   # see ./configure --help for configuration options
make check    # runs builtin unit tests
sudo make install  # set DESTDIR= to install to an alternate location
```


## LibreSSL与OpenSSL共存

LibreSSL默认安装到/usr/local目录下，OpenSSL在/usr目录下。  
执行程序，LibreSSL在/usr/local/bin，OpenSSL在/usr/bin。  
库，LibreSSL在/usr/local/lib，OpenSSL在/usr/lib。  

配置文件，LibreSSL在/usr/local/etc，OpenSSL在/etc。  

当前路径设置，显示openssl使用的是LibreSSL还是OpenSSL
```bash
$ openssl version -a
```
