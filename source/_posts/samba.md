---
title: samba配置文件共享服务
date: 2016-09-13 14:50:04
tags:
---

# 测试机配置samba服务

samba可以使我们本地跟测试机连通，从而提高开发效率
首先你得有`root`帐户，和`work`帐户

## 安装

使用`yum`安装：`yum -y install samba`

## 配置

> 使用`root`

修改`vi /etc/samba/smb.conf`，添加以下：

```
# 全局配置
workgroup = WORKGROUP
security = user


# 这下是配置work
[work]
   comment = home work
   path = /home/work
   public = no
   browseable = yes
   writable = yes
   valid users = work
```

再执行`smbpasswd -a work`添加帐户到`samba`

保存后可用`testparm`查看配置是否有问题

查看`/home/work`是否有`work`权限，没有则进入`root`添加所有者：

```bash
chown work:work -R /home/work/
```

再添加`work`的权限：

```bash
chmod -R 744 /home/work
```

执行`/etc/init.d/smb start`启动

## 本地链接测试机

### mac

打开`Finder`，按`command+k`（或者点菜单中的 前往->服务器），输入地址或者`ip`，使用`work`帐户登录，链接之后在命令行里的路径是：`/Volumes/work/`

### win

使用运行命令(`win+R`)，输入`//IP`可打开，也可以打开`我的电脑`右击选择`添加一个网络位置`，输入`//IP/work`可映射到本地为一个盘符

## 链接

* [权限](http://zhaoyuqiang.blog.51cto.com/6328846/1214718)
* [samba配置](http://blog.163.com/feng_qi_1314/blog/static/52820339201211693712365/)
