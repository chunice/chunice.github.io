---
layout: post
title:  "配置 CocoaPods 做 iOS 开发依赖管理"
date:   2016-03-18 00:00:00 +0800
categories: tool
description: CocoaPods 是 iOS开发 最常用的第三方类库管理工具，绝大部分有名的开源类库都支持 CocoaPods。
---

![cocoapods-logo](./img/cocoapods.jpg)

## 安装 Cocoapods ##


#### OSX 下使用 Ruby gem 安装 Cocoapods

Cocoapods 使用 Ruby 开发的，OSX 已经默认继承了 Ruby 环境，所以我们可以直接通过 gem 来安装 Cocoapods

{% highlight shell %}

sudo gem install cocoapods

{% endhighlight %}


#### 使用 淘宝 Ruby 镜像 安装 Cocoapods

因为某种神秘的力量 Ruby 的软件源无法正常访问，所以需要通过以下命令替换为由淘宝提供的镜像再执行`install`

```shell
gem sources --remove https://rubygem.org/
gem sources -a https://ruby.taobao.org
gem sources -l
```

## 初始化 Podfile 文件
在 Terminal 切换到项目目录，使用` pod init `命令来初始化 Podfile 文件，初始化后编辑 Podfile 文件，` vi ./Podfile `
```
platform :ios, '8.0'
use_frameworks!
target 'AlamofireDemo' do
    pod 'Alamofire', '~> 3.0'
    pod 'SwiftyJSON', :git => 'https://github.com/SwiftyJSON/SwiftyJSON.git'
end
```

platform 为当前项目所支持的系统及最低版本

use_frameworks! 为当前项目使用pods框架，(swift 必须使用 pods 框架)

pod 为当前项目所依赖的库名称

## 使用 Cocoapods 镜像源加速
又是因为某种神秘的力量，导致下载 Cocoapods 包时过慢，这时可以使用 tuna 的镜像(tuna：清华大学开源软件镜像站)

```
pod repo remove master
pod repo add master https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git
pod repo update
```

新版本的 Cocoapods 已经不允许用`pod repo add`直接添加 master 库了，但是删除 master 重新从镜像克隆：
```
cd ~/.cocoapods/repos 
pod repo remove master
git clone https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git master
```

同时必须在 Podfile 文件首行加入以下代码
```
source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'
```

## 使用 Cocoapods 安装库
```
// 安装并显示安装过程
pod install --verbose
```

每次修改了 Podfile 文件后还需要更新
```
// 更新并显示更新过程
pod update --verbose
```

## 安装或更新时不自动更新镜像源
在执行安装和更新时 Cocoapods 会自动先更新一次本地的镜像源，这样会导致安装或更新流程变慢，使用`--no-repo-update`参数会禁止自动更新镜像源
```
pod install --no-repo-update
pod update --no-repo-update
```