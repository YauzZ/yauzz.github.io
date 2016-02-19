---
layout: post
title: "VirtualBox 小规模集群管理实践.md "
description: ""
category: 
tags: [virtualbox,cluster]
---
{% include JB/setup %}


## 背景故事

上回说到咱也开始搞高大上的高并发网络服务器编程了，在那篇[博文](http://www.blogjava.net/yongboy/archive/2013/04/28/398558.html)里，作者使用了9台机器，每个绑定多个网卡来实现，这就要求我需要有一个集群来做类似的事情。

## Gentoo

我从09年开始弄Gentoo，各种发行版本里就属它最满意了。但要做集群管理的种子机，还需要有些更改。


## VirtualBox

世面上的虚拟机技术还是很多的，但由于我不能接触到硬件机器的安装，只能在之上再运行虚拟机，这样VirtualBox正好满足，而且也是我一直使用的工具，对它也了解。

现在的问题是，以前我都是模拟一两台机器，图形界面就可以搞定，现在要采用集群来做这个事情，全命令行操作是必然的事。VirtualBox可以吗？

## 高大上的CLI

[VirtualBox命令行手册](https://www.evernote.com/shard/s23/sh/ec499e66-4ff5-4f31-bc5e-1779e42230d7/e95e3a7c4dffdb9006926e9a6851ac37)，这里只有列表，很繁杂，不太好使用。

所以先抽取几个常用操作后，用脚本来简化整个管理工作。在以下的文中vboxmgr是我对VBoxManager的软连接，Gentoo #0是我的种子机。

#### clone

以新名字命名镜像后并进行注册，这是部署的第一步。

```
for i in `seq 1 10`
do  
	vboxmgr clonevm Gentoo#0 --name Gentoo_$i --register 
done
```

批量生成10个镜像，感觉真棒真奢侈。

#### unregistervm

取消注册并删除镜像

```
for i in `seq 2 10`
do  
	vboxmgr unregistervm  Gentoo_$i --delete    
done
```

#### startvm

启动镜像

```
for i in `seq 2 10`
do  
	vboxmgr startvm  Gentoo_$ --type headless    
done
```

后面那个参数是不带界面启动，如果不带得话默认就是带界面启动。

#### modifyvm 

这是对镜像参数修改的命令，主要说说几个常用的吧。

```
for i in `seq 2 10`
do  
	vboxmgr modifyvm  Gentoo_$ --name [new name] \   // 修改vm名字
								--memory <memorysize in MB> \ // 修改vm占用内存
done
```

## PSSH

[这里的的介绍](http://kumu-linux.github.io/blog/2013/08/12/pssh/)比较不错，记得原来在TB时，我们是使用自己的批量管理工具，貌似叫gm.sh，往事不堪回首啊。

有了这个工具，在把上文中部署的Gentoo镜像做一个服务器列表，操作起来就方便很多。

## 结语

准备了这么些东西，都是为了搭建一个可控的服务器集群环境，有了这个基础，erlang的开发学习一定会事半功倍啦。
