---
layout: post
title: "构建 NetworkExtension 应用——背景相关"
description: ""
category: articles
tags: [NetworkExtension]
comments: true
---

## 前言

个人水平实在有限，大多数时候只能依靠谷歌来解决编程中遇到的难题。可是国庆后各路科学上网工具逐一翻车，[蓝灯](https://github.com/getlantern/forum)(7 月份刚续费两年)整个十月份基本不可用，当前新版本可用但已经没有之前稳定、快速了。 Nydus 这种无良商家更是过分，整个团队直接消失（会员至少有一年多才到期）。中间试用过别的工具，也都并不稳定。

于是决心[自己动手,丰衣足食](https://baike.baidu.com/item/%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%EF%BC%8C%E4%B8%B0%E8%A1%A3%E8%B6%B3%E9%A3%9F)。

## 方案

方案一：
[科学上网的终极姿势:在 Vultr VPS 上搭建 Shadowsocks](https://zoomyale.com/2016/vultr_and_ss/)

方案二：
[使用 Linux 快照搭建 GFW.Press 服务器](https://gfw.press/blog/?p=30)

几乎没遇到坑。需要注意的是 Vultr 上只要建立了服务器，就会开始计费，无论是否在运行中，所以不用的服务器请直接删掉。另外 Tokyo 和 Los Angeles 的节点貌似容易被封掉，反正我建了一个节点是 ping 不通的。

知其然，更要知其所以然。

## Shadowsocks 相关

中国特色社会主义互联网发展史：

很久以前，访问网站很简单。

![whats-shadowsocks-01](https://lettleprince.github.io/images/20171113-NetworkExtension/whats-shadowsocks-01.png)

后来，[GFW](https://zh.wikipedia.org/wiki/%E9%98%B2%E7%81%AB%E9%95%BF%E5%9F%8E) 出现。

![whats-shadowsocks-02](https://lettleprince.github.io/images/20171113-NetworkExtension/whats-shadowsocks-02.png)

翻越 GFW 比直接的方案就是：墙外有一台不受限制的服务器，我们要请求或发送的数据通过这台服务器进行中转，这就需要我们通过加密来保证与墙外服务器通讯时不被 GFW 怀疑和窃听。比较常用的技术有：HTTP 代理服务、Socks 服务、VPN 服务等，其中以 SSH tunnel 的方法比较有代表性。

![whats-shadowsocks-03](https://lettleprince.github.io/images/20171113-NetworkExtension/whats-shadowsocks-03.png)

- 首先用户和墙外服务器基于 SSH 建立起一条加密的通道。
- 用户通过建立起的隧道进行代理，通过 SSH 服务器向真实的网站发起请求。
- 网站数据返回到 SSH 服务器，再通过创建好的隧道返回给用户。

SSH 本身基于 RSA 加密技术，GFW 无法从数据传输的过程中的加密数据内容进行关键词分析，避免了被重置链接的问题，但由于创建隧道和数据传输的过程中，SSH 的特征明显。GFW 也不是吃素的，它会通过特征分析，识别出 SSH 隧道然后进行干扰。

于是，[Shadowsocks](https://zh.wikipedia.org/wiki/Shadowsocks)诞生。

![whats-shadowsocks-04](https://lettleprince.github.io/images/20171113-NetworkExtension/whats-shadowsocks-04.png)

- 客户端发出的请求基于 Socks5 协议跟 ss-local 端进行通讯，由于这个 ss-local 一般是本机或路由器或局域网的其他机器，不经过 GFW，所以解决了上面被 GFW 通过特征分析进行干扰的问题。
- ss-local 和 ss-server 两端通过多种可选的加密方法进行通讯，经过 GFW 的时候是常规的 TCP 包，没有明显的特征码而且 GFW 也无法对通讯数据进行解密。
- ss-server 将收到的加密数据进行解密，还原原来的请求，再发送到用户需要访问的服务，获取响应原路返回。

不过关于 Shadowsocks 特征被识别的消息一直有，随时准备新的技术吧。

向 [clowwindy](https://github.com/clowwindy) 及后续的维护人员致敬。

## NetworkExtension 相关

[NetworkExtension](https://developer.apple.com/documentation/networkextension)是苹果提供的用于配置 VPN 和定制、扩展核心网络功能的框架。NE 框架提供了可用于定制、扩展 iOS 和 MacOS 系统的核心网络功能的 API。[Potatso](https://github.com/Potatso/Potatso) 便是使用 NE 框架实现了 Shadowsocks 代理，遗憾的是由于[种种原因](https://sspai.com/post/38909)作者删除了开源代码。

### 代码：
文章中的代码都可以从我的GitHub [`ImagePicker-Objective-C`](https://github.com/lettleprince/ImagePicker-Objective-C)找到。


1.Capabilities 设置
2.网络权限申请


Starty

So, how did everything start?