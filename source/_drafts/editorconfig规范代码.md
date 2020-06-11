---
title: editorconfig规范代码
tags: editorconfig
categories: 前端工程化
---

# 介绍
EditorConfig 可以帮助开发者在不同的编辑器和 IDE 之间定义和维护一致的代码风格。EditorConfig 包含一个用于定义代码格式的文件和一批编辑器插件，这些插件可以让编辑器读取配置文件并依此格式化代码。

<!-- more -->



## 属性

### indent_style
缩进方式，可选值：
- `tab`
- `space`

### indent_size
缩进长度，可选值：
-  整数
- `tab`

### end_of_line
换行符，可选值：
- `lf`
- `crlf`
- `cr`

### charset
文件字符编码，可选值：
- `latin1`
- `utf-8`
- `utf-16be`
- `utf-16le`
- `utf-8-bom`

### trim_trailing_whitespace
是否将行尾空格自动删除，可选值：
-  `true`
-  `false`

### insert_final_newline

是否使文件以一个空白行结尾，可选值：

-  `true`
-  `false`



### root

表明是最顶层的配置文件，发现设为 `true` 时，才会停止查找`.editorconfig` 文件。



## 个人常用配置

在项目根目录新建`.editorconfig`文件：

```
# https://editorconfig.org

root = true

[*]
charset = utf-8
indent_style = space
indent_size = 4
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
insert_final_newline = false
trim_trailing_whitespace = false
```



# 编辑器启动EditorConfig插件

- [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

- webstorm

    File > settings > Editor > Code Style > Enable EditorConfig support