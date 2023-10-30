---
layout: post
title:  "Vagrant 开发环境搭建"
date:   2016-03-10 00:00:00 +0800
categories: tool
---

![vagrant-logo](./img/vagrant.jpg)

## 安装 Vagrant ##

#### OSX 下使用 brew 安装 VirtualBox
```
brew install Caskroom/cask/virtualbox
```
#### OSX 下使用 brew 安装 Vagrant
```
brew install vagrant
```
ps: 使用 Windows 操作系统的直接搜索软件名称下载安装

## 下载并导入 box

box，你可以把它理解为镜像，每个镜像都是不同的操作系统或者是基于操作系统封装好的开发环境。比如 CentOS，Ubuntu 等等，你可以基于它们去创建自己版本的 Box，比如在操作系统的基础上配置好开发环境，然后把它重新打包成 Box 。
#### 直接下载并添加镜像
```
vagrant box add 镜像名称
```

这样会直接在[https://vagrantcloud.com] 搜索并下载到本地
但是因为某种诡异的网络问题，我建议你下载本地后手动导入。
这是宁皓网下载了几个常用 box 并传到了百度云盘[http://pan.baidu.com/s/1qWmc18S]

```
// vagrant box add 镜像名称 镜像绝对路径
vagrant box add centos7 /Develop/vagrant_box/centos7.box
```

你还可以通过命令` vagrant box list `来查看本地有多少镜像，以及通过命令` vagrant box remove 镜像名称 `来删除已经导入的镜像

## 初始化环境
切换到代码所在目录，执行
```
vagrant init centos7
```
这样就把 centos7 虚拟开发环境初始化在了这个目录

## 修改 Vagrantfile 文件配置网络环境
使用`init`初始化环境后，在当前目录会出现 Vagrantfile 的脚本文件，这是一个 Ruby 程序文件也是 Vagrant 配置文件，我们可以通过这个文件直接配置网络环境和镜像启动后首先运行哪些命令
```ruby
# 把本地目录 /../data 与服务器的 /home/vagrant 桥接
config.vm.synced_folder "../data", "/home/vagrant"

# 三选一
# 把服务器的80端口转到本机的8080端口
config.vm.network "forwarded_port", guest: 80, host: 8080
# 让 Vagrant 虚拟机向一台真的机器那样去路由器获取 IP（局域网其他机器也能访问到）
config.vm.network "public_network"
# 把 Vagrant 虚拟机虚拟成私有网络，只有本机能访问到（不推荐，私有网络会产生配置文件，稍后把 vagrant 镜像再封装会产生其他工作）
config.vm.network "private_network", ip: "192.168.33.10"

# 设置虚拟机最大内存
config.vm.provider "virtualbox" do |vb|
    # 是否开启 VirtualBox GUI调试
    vb.gui = true

    # 设置当前 vagrant 虚拟机最大内存
    vb.memory = "1024"
end

# 设置虚拟机启动后执行哪些命令或 shell 脚本
config.vm.provision "shell", inline: <<-SHELL
    sudo lnmp restart
    sudo lnmp status
SHELL
```
这些配置文件不需要你自己写入，已经在脚本中写好，你只需要去掉注释并修改成你想要的样子，保存就可以了

## 启动环境
```
vagrant up
```

## ssh 连接环境
```
ssh vagrant@127.0.0.1:2222
```
Vagrant 的默认帐号密码都是`vagrant`<br>
使用 Windows 的需要通过第三方工具连接 ssh，如 Xshell, putty等。强烈推荐 Xshell

## 镜像再封装
```
vagrant package
```
把配置好的镜像重新封装成一个 box 必须先执行命令`vagrant halt`关闭虚拟机，再在当前目录执行以上命令，然后什么都不需要做，等待一会儿就可以在当前目录下发现一个 package.box 文件了

## 其他常用命令
```
vagrant init      #初始化
vagrant up        #启动虚拟机
vagrant halt      #关闭虚拟机
vagrant reload    #重启虚拟机
vagrant ssh       #ssh连接
vagrant status    #查看虚拟机状态
vagrant destroy   #销毁虚拟机
```





