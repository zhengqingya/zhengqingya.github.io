---
layout: post
title: 'Docker之Windows10安装教程（DockerToolbox）'
date: 2019-01-05
categories: 技术
tags: Docker
---

**使用DockerToolbox安装Docker**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426231946470.png)
**一、DockerToolbox下载链接：** https://pan.baidu.com/s/1P-UYxShou4hCx9yWxage_g 
提取码：4j44
 
**二、下载完之后双击进行安装**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232019246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232123707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232140208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232209535.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232226480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232242269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232350269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232455597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
**三、完成之后桌面会出现如下3个图标**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426232559794.png)
双击中间那个图标( **注：** 用管理员身份运行哦 ) ，如果出现如下情况，可能是因为之前已经安装过git出现的问题~

windows 正在查找bash.exe。如果想亲自查找文件，请点击“浏览”。

或
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426233044779.png)
**解决方法：**

右击图标选择属性 --> 修改目标

原本目标：
```java
C:\Windows\WinSxS\amd64_microsoft-windows-lxss-bash_31bf3856ad364e35_10.0.14393.0_none_6d0fca0ad344bc62\bash.exe --login -i "F:\IT_zhengqing\soft\Docker\Docker Toolbox\start.sh" 
```
因为个人之前git的安装路径不在c盘所以需要修改git的路径

最后修改如下：
```java
F:\IT_zhengqing\soft\Git\Git\bin\bash.exe --login -i "F:\IT_zhengqing\soft\Docker\Docker Toolbox\start.sh"
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426233430851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
然后再次双击图标 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426233923950.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426233903605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
如果一直没有反应，那是因为我们要下载的这个镜像文件已经有了只是不在C盘的目录里，就需要我们手动去拷贝一下~

将安装目录里的boot2docker.iso镜像文件复制一份到C:\Users\Administrator\.docker\machine\cache下，如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426235502678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

**注：** 这里先断网再双击start.sh  启动重新加载 （断网目的：让它不再去下载镜像文件）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426235147212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
如下界面成功！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427000341363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
第一次启动服务需要创建一台虚拟机，然后等待一会儿就会出现如下界面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427000422622.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
如果IP...这里一直没有跳转过去，就需要连接网络哦~，（当然我这里很顺利直接跳过去的，哈哈）

最后安装成功之后，如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427000651838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
**四、优化 - 镜像加速**

start.sh启动的服务窗口不要关，使用git另开一个窗口连接名叫default的虚拟机，执行命令：**docker-machine ssh default** 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427001451901.png)
这里测试连接成功，如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427001352399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

---

**注：** 在虚拟机中要运行容器，下载镜像，而镜像的注册中心在国外需要翻墙且很慢。因此我们可以使用阿里云镜像加速器来下载。

1. 先删除已有的default虚拟机，再重新创建一个配置了镜像加速器的虚拟机。
2. 阿里云镜像加速器申请地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427010232555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)  
3. 在docker服务窗口输入以下命令
   ```java
   docker-machine stop default -- 停止虚拟机   
   docker-machine rm default  --删除虚拟机  
    ```
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427002457540.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
5. **注：** 这里在创建虚拟机之前先断网！
   ```java
    docker-machine create --engine-registry-mirror=镜像加速器地址 -d virtualbox default  --创建一个叫default的虚拟机
   ```
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427010548897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
   当又出现IP...就需要再次联网了哦~
   
   如下成功！！！![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427010701996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

**完整的一条里程碑如下：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427004059225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
**五、测试**

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019042701111059.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
**六、docker命令**

```java
#开发模式:
docker-compose -f docker-compose.yml -f docker-compose.dev.yml build
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up

#查看所有容器:
docker ps -a 
#查看所有镜像:
docker images
#删除所有的容器:
docker rm $(docker ps -aq) 

#删除容器应用: 
docker ps -a --format "{{.ID}}" | foreach {
    docker stop $_
    docker rm $_
}
 
#删除本地容器镜像:
docker images --format "{{.ID}}" | foreach {
   docker rmi stop $_
}
```

