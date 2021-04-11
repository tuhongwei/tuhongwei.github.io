---
title: 记录一次使用 yarn patch 遇到的问题
date: 2021-04-10 22:51:05
author: Twittytop
categories:
- 教程
tags:
- Yarn
keywords: Yarn v2 patch
typora-root-url: ..\..
---

最近在修改 node_modules 里的文件时（具体可以参考我的另一篇文章—如何修改 node_modules 里的文件），发现了 yarn patch 这个特性，这是 v2 版本的一个新特性，于是想动手实验一番，结果报了如下错误：

![screenshot12](/images/blog/modify-node_modules/screenshot12.jpg)



意思是说在我的 lockfile 文件里面找不到我的这个依赖包，我一脸黑人问号， 明明刚安装了这个包，为什么会找不到呢？

![img](/images/blog/yarn-patch/confuse.jpg)



于是我重新 `yarn install` 一下，发现了如下错误（用的是vue-cli4）：

![screenshot1](/images/blog/yarn-patch/screenshot1.jpg)

猜想会不会是因为 vue-loader-v16@16.2.0 这个包的镜像问题导致最后安装没有成功，然后搜了一下这个包，发现没有这个版本的包依赖，又继续搜了一下是哪个包依赖了 vue-loader-v16，发现了这个

![screenshot2](/images/blog/yarn-patch/screenshot2.jpg)

"npm:"  是什么鬼，查了一下，发现这是一个别名，也就是说，安装 vue-loader-v16 的时候会去安装 vue-loader@^16.1.0，难道是 v2 版本的 yarn 安装的时候识别不出这种别名吗？因为我发现用 v1 版本的 yarn 安装的时候是没问题的。于是我立马试验了一番，另起一个工程，添加 一个包别名，是能安装成功的，证明 v2 版本的 yarn 识别这种别名是没有问题的。那到底是什么原因导致没有安装成功呢？



仔细看最开始的报错，注意到这段话， "try to make an install to update your resolutions"，查看 v1 版本的 yarn.lock 文件时，发现的是这样

![screenshot3](/images/blog/yarn-patch/screenshot3.jpg)



安装包的地址应该是保存在 resolved 字段里面的，但是为什么这里的报错却提示是 resolutions 呢？难道 v2 版本的 yarn 换了字段名吗？于是我删掉了 yarn.lock 文件，重新 `yarn install` 一下，结果安装成功了，然后我查看了一下 v2 版本的 yarn.lock 文件

![screenshot4](/images/blog/yarn-patch/screenshot4.jpg)

发现里面的字段名的确是 resolution ，由此证实了我的猜想。

所以最开始的安装没成功是因为 yarn 试图去找 resolution 对应的安装包地址，结果没有找到而报错。

由此得出结论：**当把 yarn 升级到 v2 时，要先删除原来的 yarn.lock 文件，然后再执行 `yarn install`。**

