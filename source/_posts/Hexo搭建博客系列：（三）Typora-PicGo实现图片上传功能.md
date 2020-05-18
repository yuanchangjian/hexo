---
title: Hexo搭建博客系列：（三）Typora+PicGo实现图片上传功能
date: 2020-05-12 18:25:00
categories: Hexo搭建博客系列
tags: 
    - Hexo
---

# 前言

编写markdown时经常会有插入图片的需求，而粘贴的图片的地址为本地地址，部署时是无法查看的。办法总比问题多，下面教大家使用Typora+PicGo+GitHub图床给markdown愉快的添加图片。

<!-- more -->



# 安装

自行下载安装如下软件：

* [Typora](https://www.typora.io/)
* [PicGo](https://github.com/Molunerfinn/PicGo/releases)

## Github图床

* 新建repository，命名为`cloudImage`
* 添加访问token，复制并保存token（只会出现一次）

![image-20200518162328069](https://yuanchangjian.github.io/cloudImage/images/20200518162329.png)

![image-20200518162420210](https://yuanchangjian.github.io/cloudImage/images/20200518162424.png)

​	![image-20200518162120925](https://yuanchangjian.github.io/cloudImage/images/20200518162122.png)

​	![image-20200518162213723](https://yuanchangjian.github.io/cloudImage/images/20200518162215.png)



# 配置

## 配置typora

打开typora，选择左上角`文件`，选择`偏好设置`，配置成如下所示：

![image-20200518155153969](https://yuanchangjian.github.io/cloudImage/images/20200518160024.png)



## 配置PicGo

### 图床设置

根据上述新建的GitHub图床进行配置（我这里在仓库内创建了一个`images`目录存储图片）。

![image-20200518162929836](https://yuanchangjian.github.io/cloudImage/images/20200518162932.png)



### 开启时间戳重命名

防止GitHub重名上传失败。

 ![image-20200518163355105](https://yuanchangjian.github.io/cloudImage/images/20200518163356.png)



# 使用说明

将本地图片放入Typora时，会出现上传地图选项，点击上传图片即可。这里我演示一下图片地址的变化：

![image-20200518164240350](https://yuanchangjian.github.io/cloudImage/images/20200518164244.png)

上传后图片地址：

![image-20200518164334296](https://yuanchangjian.github.io/cloudImage/images/20200518164336.png)



