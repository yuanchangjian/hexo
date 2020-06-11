---
title: ESLint使用指南
tags: ESLint
categories: 前端工程化
date: 2020-06-11 22:24:34
---




# 前言

`ESLint`是最流行的`JavaScript Linter`。

> Linter 是检查代码风格/错误的小工具。其他类似的 Linter 工具还有：TSLint、stylelint。

它包含三个功能：

（1）check syntax

（2）find problems

前两个可以统称为 Code-quality rules，例如 [no-unused-vars ](http://eslint.org/docs/rules/no-unused-vars)规则。

（3）enforce code style

最后一个可以称为 **Formatting rules** ，例如 [keyword-spacing](http://eslint.org/docs/rules/keyword-spacing) 规则。



<!-- more -->

# 安装依赖

```
yarn add eslint --dev
```



# 参数说明

有两种主要的方式来配置 `ESLint`：

* 代码注释（一般由编辑器修复，这种情况应考虑`.eslintrc.*`的配置，尽量不要有代码注释）
* 配置文件[`.eslintrc.*`](https://eslint.bootcss.com/docs/user-guide/configuring#configuration-file-formats)或`package.json`中指定`eslintConfig`字段

有很多信息可以配置：

- **Environments** - 指定脚本的运行环境。每种环境都有一组特定的预定义全局变量。
- **Globals** - 脚本在执行期间访问的额外的全局变量。
- **Rules** - 启用的规则及其各自的错误级别。

{% note danger %} 

非常用参数未详细列举出来，有兴趣的自行[官网](https://eslint.bootcss.com/docs/user-guide/configuring)学习。

{% endnote %}

## parseOptions - 指定解析选项

* `ecmaVersion` - 默认设置为 3，5（默认）， 你可以使用 6、7、8、9 或 10 来指定你想要使用的 ECMAScript 版本。你也可以用使用年份命名的版本号指定为 2015（同 6），2016（同 7），或 2017（同 8）或 2018（同 9）或 2019 (same as 10)

* `sourceType` - 设置为 `"script"` (默认) 或 `"module"`（如果你的代码是 ECMAScript 模块)。
* `ecmaFeatures` \- 这是个对象，表示你想使用的额外的语言特性
    * `globalReturn` - 允许在全局作用域下使用 `return` 语句
    * `impliedStrict` - 启用全局 [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) (如果 `ecmaVersion` 是 5 或更高)
    * `jsx` - 启用 [JSX](http://facebook.github.io/jsx/)
    * `experimentalObjectRestSpread` - 启用实验性的 [object rest/spread properties](https://github.com/sebmarkbage/ecmascript-rest-spread) 支持。

{% note danger %} 

支持 ES6 语法并不意味着同时支持新的 ES6 全局变量或类型（比如 `Set` 等新类型）。对于 ES6 语法，使用 `{ "parserOptions": { "ecmaVersion": 6 } }`；对于新的 ES6 全局变量，使用 `{ "env":{ "es6": true } }`。`{ "env": { "es6": true } }` 自动启用es6语法，但 `{ "parserOptions": { "ecmaVersion": 6 } }` 不自动启用es6全局变量。

{% endnote %}

```
{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    }
}
```



## parser - 指定解析器

- [Espree](https://github.com/eslint/espree)
- [Esprima](https://www.npmjs.com/package/esprima)
- [Babel-ESLint](https://www.npmjs.com/package/babel-eslint) - 一个对[Babel](https://babeljs.io/)解析器的包装，使其能够与 ESLint 兼容。
- [@typescript-eslint/parser](https://www.npmjs.com/package/@typescript-eslint/parser) - 将 TypeScript 转换成与 estree 兼容的形式，以便在ESLint中使用。

{% note danger %} 

Babel-ESLint官方说明了不要因为项目中使用了babel就使用该解析器。当ESLint解析器不能够支持某些实验性功能或者类型（Flow），才考虑使用Babel-ESLint解析器。

{% endnote %}

例如，下面的配置指定了 Esprima 作为解析器：

```
{
    "parser": "esprima"
}
```



## env - 指定环境

一个环境定义了一组预定义的全局变量。常用的环境如下所示:

- `browser` - 浏览器环境中的全局变量。
- `node` - Node.js 全局变量和 Node.js 作用域
- `es6` - 启用除了 modules 以外的所有 ECMAScript 6 特性（该选项会自动设置 `ecmaVersion` 解析器选项为 6）
- `mocha` - 添加所有的 Mocha 测试全局变量。
- `jquery` - jQuery 全局变量
- ...

例如，以下示例启用了 browser 和 Node.js 的环境：

```
{
    "env": {
        "browser": true,
        "node": true
    }
}
```



## globals - 指定全局变量

当访问当前源文件内未定义的变量时，[no-undef](https://eslint.bootcss.com/docs/rules/no-undef) 规则将发出警告。如果你想在一个源文件里使用全局变量，推荐你在 ESLint 中定义这些全局变量，这样 ESLint 就不会发出警告了。

对于每个全局变量键，将对应的值设置为 `"writable"` 以允许重写变量，或 `"readonly"` 不允许重写变量。例如：

```
{
    "globals": {
        "var1": "writable",
        "var2": "readonly"
    }
}
```

可以使用字符串 `"off"` 禁用全局变量。例如，在大多数 ES2015 全局变量可用但 `Promise` 不可用的环境中，你可以使用以下配置:

```
{
    "env": {
        "es6": true
    },
    "globals": {
        "Promise": "off"
    }
}
```

{% note danger %} 

**注意：**要启用[no-global-assign](https://eslint.bootcss.com/docs/rules/no-global-assign)规则来禁止对只读的全局变量进行修改。

{% endnote %}



## plugins - 配置插件

例如，指定`eslint-plugin-vue`插件检测vue代码：

```
{
    "plugins": [
        "vue"
    ]
}
```



## extends - 规则继承

一个配置文件可以被基础配置中的已启用的规则继承。

`extends` 属性值可以是：

- 指定配置的字符串(配置文件的路径、可共享配置的名称、`eslint:recommended` 或 `eslint:all`)
- 字符串数组：每个配置继承它前面的配置



例如，继承eslint推荐配置`eslint:recommended`，然后添加自定义的规则：

```
{
    "extends": "eslint:recommended"，
    "rules": {
        // enable additional rules
        "indent": ["error", 4],
    }
}
```



## eslintignore - 忽略文件

同gitignore。例如，忽略node_modules目录的检测：

```
node_modules
```



# 构建Lint工作流

下面以vue项目为例，具体插件和设置还需根据项目的依赖来配置。

## 生成配置文件

```
eslint --init
```

![image-20200610221320271](https://yuanchangjian.github.io/cloudImage/images/20200610230949.png)



## 添加规则文件

```
module.exports = {
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:vue/essential"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "plugins": [
        "vue"
    ],
    "rules": {
    	// 添加自定义规则，同一团队代码风格
    }
};
```



### 添加忽略文件`.eslintignore`

根据项目添加忽略目录，如：

```
dist
node_modules
.idea
```



## [集成](https://eslint.bootcss.com/docs/user-guide/integrations)

### 编辑器

* Visual Studio Code: [ESLint Extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

* WebStorm

    File > settings > Languages & Frameworks> Javascript > Code Quality Tools > ESLint



### 构建工具

#### Rollup

##### 安装rollup-plugin-eslint

```
yarn add rollup-plugin-eslint --dev
```

##### 用法

```
import { eslint } from "rollup-plugin-eslint";
 
export default {
  input: "main.js",
  plugins: [
    eslint()
  ]
};
```



#### Webpack

#### 安装eslint-loader

```
yarn add eslint-loader --dev
```

#### 用法

```
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ['babel-loader', 'eslint-loader'],
      },
    ],
  },
  // ...
};
```



# Git hooks

## 安装依赖

```
yarn add -D husky lint-staged pretty-quick	
```



## 配置package.json

```
    "scripts": {
		...其他脚本命令
        "precommit": "pretty-quick --staged"
    },
    "lint-staged": {
        "src/**/*.{js,jsx}": [
            "eslint --fix",
            "git add"
        ]
    },
    "husky": {
        "hooks": {
            "pre-commit": "lint-staged"
        }
    },
```



# 个人常用规则

```
{
    rules: {
        indent: ['error', 4]
    }
}
```



# 参考

* [ESLint官网](https://eslint.bootcss.com/)
* [VSCode 使用 ESLint + Prettier 来统一 JS 代码](https://www.cnblogs.com/xjnotxj/p/10828183.html)
* [ESLint 工作原理探讨](https://zhuanlan.zhihu.com/p/53680918)
* [AlloyTeam 规范](https://github.com/AlloyTeam/eslint-config-alloy/blob/master/README.zh-CN.md)
* [从零构建前端 Lint 工作流](https://segmentfault.com/a/1190000022881634)
* [Git Precommit Hook](https://coderwall.com/p/zq8jlq/eslint-pre-commit-hook)

