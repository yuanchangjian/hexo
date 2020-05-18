---
title: Hexo搭建博客系列：（二）自定义样式优化主题
date: 2020-05-12 13:42:08
categories: Hexo搭建博客系列
tags: 
    - Hexo
---

# 前言

本教程是我对Next主题的一些自定义样式修改，主要通过两种方式：修改`主题配置文件`和自定义样式。

<!-- more -->

## 主题配置文件

### 隐藏文章更新日期

```
post_meta:
  item_text: true
  created_at: true
  updated_at:
    enable: false
    another_day: true
  categories: true
```

### 友情链接行内布局

```
links_settings:
  icon: fa fa-link
  title: Links
  # Available values: block | inline
  layout: inline
```

### footer图标颜色

```
footer:
  ...
  icon:
    # Icon name in Font Awesome. See: https://fontawesome.com/icons
    name: fa fa-heart
    # If you want to animate the icon, set it to true.
    animated: false
    # Change the color of icon, using Hex Code.
    color: "#000000"'
  ...
```



## 自定义样式

在主题目录的`/source/css`目录下新建`_custom.styl`文件。内容如下所示：

```
/*
* 文章目录显示多级标题,文章展示所有二级菜单
*/
//显示一级标题下的子标题
.post-toc .nav .nav-level-1 > .nav-child {
   display: block;
}
//显示二级标题下的子标题
/*.post-toc .nav .nav-level-2 > .nav-child {
   display: block;
}/*
//显示三级标题下的子标题
/*.post-toc .nav .nav-level-3 > .nav-child {
   display: block;
}*/

/*
* 导航栏菜单右对齐
*/
.menu {
  text-align: right;
}
```



