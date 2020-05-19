---
title: 语义化版本控制规范（SemVer）
date: 2020-05-19 18:04:53
categories: 版本管理
tags:
	- SemVer
---

# 简介

`semver` 是 [语义化版本（Semantic Versioning）规范](http://semver.org/lang/zh-CN/) 的一个实现，目前是由 `npm` 的团队维护，实现了版本和版本范围的解析、计算、比较。下面列举一个`npm`下`vue`的版本历史。

<!-- more -->

![image-20200519181754033](https://yuanchangjian.github.io/cloudImage/images/20200519192502.png)



# 版本格式

1. 主版本号：当你做了不兼容的 API 修改。（如：V2.6.11 -> V3.0.0-alpha.0）
2. 次版本号：当你做了向下兼容的功能性新增。（如：V2.5.22 -> V2.6.0-beta.1）
3. 修订号：当你做了向下兼容的问题修正。（如：V2.6.10 -> V2.6.11）



# 先行版本

先行版本表示这个版本并非稳定，可以作为发布正式版之前的版本，格式是在修订版本号后面加上一个连接号`（-）`，再加上一连串以点（.）分割的标识符，标识符可以由英文、数字和连接号（[0-9A-Za-z-]）组成。

```
2.6.0-beta.1
3.0.0-alpha.0
1.0.0-0.3.7
1.0.0-x.7.z.92
...
```

以下是一些常见的先行版本号名称：

- alpha：是内部测试版,一般不向外部发布,会有很多Bug.一般只有测试人员使用。
- beta：也是测试版，这个阶段的版本会一直加入新的功能。在Alpha版之后推出。
- rc：（Release　Candidate)  系统平台上就是发行候选版本。RC版不会再加入新的功能了，主要着重于除错。



# 版本编译元数据（可选）

版本编译元数据可以被标注在修订版或先行版本号之后，先加上一个`加号`再加上一连串以句点分隔的标识符来修饰。当判断版本的优先层级时，版本编译元数据可被忽略。

```
1.0.0-alpha+001
1.0.0+20130313144700
1.0.0-beta+exp.sha.5114f85
```



# 定义依赖版本号

在 [npm](https://npmjs.com/) 的依赖的规则中，还有 `~`、`>`、`<`、`=`、`>=`、`<=`、`-`、`||`、`x`、`X`、`*` 等符号；当使用 `npm install XX` 时，被安装的依赖的版本号前会默认加上 `^` 符号。

* `^` ：表示同一主版本号中，不小于指定版本号的版本号

```
 `^2.2.1` 对应主版本号为 2，不小于 `2.2.1` 的版本号，比如 `2.2.1`、`2.2.2`、`2.3.0` ,主版本号固定
// 当该依赖有最新版本时(eg:2.3.3)，npm install 会安装最新的依赖
```

* `~` ：表示同一主版本号和次版本号中，不小于指定版本号的版本号

```
 `~2.2.1` 对应主版本号为 2，次版本号为 2，不小于 `2.2.1` 的版本号，比如 `2.2.1、2.2.2`，主版本号和次版本号固定
```

常用的就是上述两种情况，拿vue做个例子：

* 项目兼容vue发布的此版本和补丁包版本："vue": "^2.5.0"
* 项目只兼容vue发布的补丁版本："vue": "~2.5.0"（发布2.6.0不会更新）



{% note warning %} 

**npm 中 package-lock.json 的一些坑**

在 npm install 后，会生成一个 package-lock.json 文件用于保存当前安装依赖的各种来源及版本号。

在 npm 5.4.2版本后，package-lock.json 的变动规则：

- 当在 install dependency 的指定版本时，会自动更新 package-lock.json 文件中该 dependency 的 version 到指定的 version
- 当在 install dependency 的范围版本时，当前的 version 低于or等于 package-lock.json 文件中对应的 dependency 的 version 时，会安装 package-lock.json 中的 version；

```
package.json
"antd": "^3.6.1", // eg：最新版本是 3.9.4

package-lock.json
"antd": "3.7.1",

执行npm install 会安装 3.7.1 版本
```

如果高于 package-lock.json 中对应的 dependency 的 version 时，会安装当前范围版本号中最高的版本，会更新 package-lock.json 文件中对应的版本号；

若版本使用`^`匹配符，想`临时`跳到某个固定版本，可以如下所示安装：

```
npm install --save antd@3.6.1
```

若要固定版本，可以先删除`package-lock.json`，将`package.json`将antd改为`3.6.1`，不使用`匹配符`，执行`npm install`

{% endnote %}



### npm包发布

通常我们发布一个包到npm仓库时，我们的做法是先修改 `package.json` 为某个版本，然后执行 `npm publish` 命令。手动修改版本号的做法建立在你对Semver规范特别熟悉的基础之上，否则可能会造成版本混乱。npm 考虑到了这点，它提供了相关的命令来让我们更好的遵从Semver规范：

- 升级补丁版本号：npm version patch
- 升级小版本号：npm version minor
- 升级大版本号：npm version major

当执行 `npm publish` 时，会首先将当前版本发布到 `npm registry`，然后更新 `dist-tags.latest` 的值为新版本。

当执行 `npm publish --tag=next` 时，会首先将当前版本发布到 `npm registry`，并且更新 `dist-tags.next` 的值为新版本。这里的 next 可以是任意有意义的命名（比如：v1.x、v2.x 等等）



# 参考

* [语义化版本 2.0.0](https://semver.org/lang/zh-CN/)
* [semver 语义化版本规范](https://www.jianshu.com/p/a7490344044f)
* [Semver(语义化版本号)扫盲](https://segmentfault.com/a/1190000014405355)