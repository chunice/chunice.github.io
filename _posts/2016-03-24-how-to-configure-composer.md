---
layout: post
title:  "配置 Composer 做 PHP 开发依赖管理"
date:   2016-03-24 00:00:00 +0800
categories: tool
---

![composer-logo](./img/composer.jpg)

## 安装 Composer

### 局部安装
在命令行输入以下命令后，等待片刻就能在当前目录下发现 composer.phar 文件

然后你就可以用 `php composer.phar` 命令来使用 Composer 了！
```
php -r "readfile('https://getcomposer.org/installer');" | php
```
由于某种神秘的力量，通过以上命令安装过程极慢的话，可以使用以下命令安装(phpcomposer 提供的 composer 安装镜像)
```
php -r "readfile('http://install.phpcomposer.com/installer');" | php
```
使用命令行局部安装 composer 同时支持 Windows/OSX/Linux 系统，但是必须保证当前系统已经安装了 php 并且包含在了系统环境变量 PATH 下，你可以通过在命令行输入`php -v`来确认是否安装了 php。

### 全局安装（推荐）
全局安装即把 Composer 安装到系统路径，可以直接在命令行使用`composer`，而局部安装则是把 Composer 安装到项目路径，只能使用`php composer`

#### Windows 系统
把通过局部安装的命令下载后的`composer.phar`放到一个你喜欢的目录下，在同目录下新建`composer.bat`文件
```bat
@php "%~dp0composer.phar" %*
```
然后把 Composer 的所在目录加入到系统环境变量中

#### OSX 或 Linux 系统
把通过局部安装的命令下载后的`composer.phar`文件移动到`/usr/local/bin/`目录下面并改名去掉后缀
```
sudo mv composer.phar /usr/local/bin/composer
```

### 使用 Composer 全量镜像加速
由于某种神秘的力量，你使用 Composer 的过程中会变得很慢，所以使用`http://pkg.phpcomposer.com/`提供的 Composer 中国全量镜像会让速度变得很快
#### 局部安装
使用命令行进入到 composer 所在的项目目录，执行以下命令
```
composer config repo.packagist composer https://packagist.phpcomposer.com
```

#### 全局安装（推荐）
直接打开命令行，执行以下命令
```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```