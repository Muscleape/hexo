---
title: centos7部署搭建golang环境
date: 2017-03-15 15:59:59
categories: golang
tags: [Linux,CentOS7,GoLang]
---
<Excert in index | 首页摘要>
CentOS7中部署GoLanguage运行环境
<!-- more -->
<the rest of contents | 余下全文>

> 下载go-linux-64位源码包，手动wget安装，不推荐yum安装（yum中版本太低）

```
[root@blog ~]# wget http://www.golangtc.com/static/go/1.8/go1.8.linux-amd64.tar.gz
```

> 下载后的源码压缩包文件如下：

```
-rw-r--r--   1 root   root    89803580 2月  17 05:01 go1.8.linux-amd64.tar.gz
```
> 解压二进制文件到/usr/local目录

```
[root@blog ~]# tar -xzf go1.8.linux-amd64.tar.gz -C /usr/local
```

> 解压目标目录下

```
[root@blog local]# pwd
/usr/local
[root@blog local]# ll
总用量 4
drwxr-xr-x.  2 root root    6 6月  10 2014 bin
drwxr-xr-x.  2 root root    6 6月  10 2014 etc
drwxr-xr-x.  2 root root    6 6月  10 2014 games
drwxr-xr-x  11 root root 4096 2月  17 03:29 go

```
> 解压源码文件目录结构

```
[root@blog go]# pwd
/usr/local/go
[root@blog go]# ll
总用量 144
drwxr-xr-x  2 root root  4096 2月  17 03:27 api
-rw-r--r--  1 root root 33243 2月  17 03:27 AUTHORS
drwxr-xr-x  2 root root    39 2月  17 03:29 bin
drwxr-xr-x  4 root root    35 2月  17 03:29 blog
-rw-r--r--  1 root root  1366 2月  17 03:27 CONTRIBUTING.md
-rw-r--r--  1 root root 45710 2月  17 03:27 CONTRIBUTORS
drwxr-xr-x  8 root root  4096 2月  17 03:27 doc
-rw-r--r--  1 root root  5686 2月  17 03:27 favicon.ico
drwxr-xr-x  3 root root    17 2月  17 03:27 lib
-rw-r--r--  1 root root  1479 2月  17 03:27 LICENSE
drwxr-xr-x 14 root root  4096 2月  17 03:29 misc
-rw-r--r--  1 root root  1303 2月  17 03:27 PATENTS
drwxr-xr-x  7 root root    82 2月  17 03:29 pkg
-rw-r--r--  1 root root  1399 2月  17 03:27 README.md
-rw-r--r--  1 root root    26 2月  17 03:27 robots.txt
drwxr-xr-x 46 root root  4096 2月  17 03:27 src
drwxr-xr-x 17 root root  8192 2月  17 03:27 test
-rw-r--r--  1 root root     5 2月  17 03:27 VERSION

```

> 使用vi在环境变量配置文件/etc/profile中增加如下内容

```
[root@blog go]# vi /etc/profile
```
> 添加golang的环境变量,GOPATH，在PATH中添加golang的目录

```
#GO GOPATH
export GOPATH=/home/golang

export PATH=$ANT_HOME/bin:$MAVEN_HOME/bin:$GRADLE_HOME/bin::$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH:/usr/local/go/bin
```

> 使profile配置立即生效

```
[root@blog ~]# source /etc/profile
```

> 查看Go版本

```
[root@blog ~]# go version
go version go1.8 linux/amd64
```

> 查看GO的环境变量

```
[root@blog ~]# go env
GOARCH="amd64"
GOBIN=""
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="linux"
GOOS="linux"
GOPATH="/home/golang"
GORACE=""
GOROOT="/usr/local/go"
GOTOOLDIR="/usr/local/go/pkg/tool/linux_amd64"
GCCGO="gccgo"
CC="gcc"
GOGCCFLAGS="-fPIC -m64 -pthread -fmessage-length=0 -fdebug-prefix-map=/tmp/go-build410979094=/tmp/go-build -gno-record-gcc-switches"
CXX="g++"
CGO_ENABLED="1"
PKG_CONFIG="pkg-config"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
```