---
layout: post
title: '【Ngrok内网穿透】-natapp安装使用教程'
date: 2019-06-06
categories: 技术
tags: 微信开发
---

**Ngrok**：可实现内网穿透（将内网的服务器映射到外网给别人访问）
**natapp**：全面支持HTTPS协议以及本地SSL证书,支持WSS协议.同时支持HTTP/2 WEB协议,支持微信小程序本地开发.
全面自动支持泛子域名与访客真实IP地址.

#### 一、natapp下载：https://natapp.cn/   
这里根据自己的系统选择下载
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603112555373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
#### 二、下载完成之后解压双击运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603114958787.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603115333975.png)
> **温馨小提示**：这里也可配置系统环境变量Path，通过cmd执行命令natapp运行
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603115720294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603115835131.png)

如上显示，这里需要单独去natapp官网创建隧道获取相应authtoken才能成功运行！

#### 三、创建隧道获取authtoken
官网上有很多隧道，这里我选择注册免费隧道~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603122655254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603122813749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
注册完成之后进入购买隧道，点击免费隧道
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603123247936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

> **温馨小提示**：这一步需要实名认证
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603123355384.png)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603123546895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
> 然后登录支付宝进行授权即可
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603123736101.png)

认证成功之后，再次进入购买隧道，点击免费隧道，进入购买
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019060312435892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
购买成功之后，在页面下方即可获取相应的authtoken！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603124518938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
#### 四、运行测试
在natapp的安装目录下打开cmd执行如下命令  （如果配置了系统环境变量可直接cmd执行相应命令，即不必到natapp安装路径下）
```
natapp -authtoken yourauthtoken
```
> 注：yourauthtoken为自己的authtoken哦~

如下映射成功~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603133558625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
最后启动自己的本地项目，测试访问成功！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603141802732.png)
