---
title: devDependencies与dependencies的区别
date: 2021-4-15
tags:
  - npm
  - webpack
categories:
  - npm
---

# devDependencies 与 dependencies 有什么区别？

## devDependencies

用字面来翻译就是“开发依赖”，就是一些在开发时候才会用到的工具包，例如：webpack，chalk，fs-extra，opn 等项目上线后不需依赖相关代码运行的工具。

## dependencies

就是“依赖”，原则上来说这个字段的工具包只允许放生产环境中项目所使用的依赖，例如：Vue，React，axios 等相关代码缺失会导致项目运行错误的工具。

## 不加 --save --save-dev 有什么后果？

在 npm 5 之前，npm install xxx 包的相关信息不会被写入到 package.json 里去，仅将源码下载到 node_modules 里提供当前开发。

在 npm 5 之后，--save 是作为默认参数添加，即 npm install xxx 包的相关信息也会被记录到 dependencies 字段去。但想添加到 devDependencies 仍然需要 --save-dev 参数。

## 依赖混乱有什么后果？

什么是依赖混乱？依赖混乱就是将本应该在生产环境的包放进开发依赖，将开发环境的包放进生产依赖。

### 1. 生产环境的包放进开发依赖

具体就例如：将 Vue 放进 devDependencies，这样做的后果是什么？分两种情况：

- 下载项目源码，然后运行 npm install。这种情况无论 Vue 在哪个字段里都会被正确下载到 node_modules 里。
- 作为 npm 包发布，然后 npm install xxx 的形式下载。这种情况只会下载 dependencies 的依赖，其余依赖将会无视处理，这样别人下载你的 npm 包运行时，就会找不到 xxx 而出错。

### 2. 开发环境的包放进生产依赖

具体就例如：将 fs-extra jest 放进 dependencies，这样做的后果是什么？分两种情况：

- 下载项目源码，然后运行 npm install。这种情况无论 fs-extra jest 在哪个字段里都会被正确下载到 node_modules 里。
- 作为 npm 包发布，然后 npm install xxx 的形式下载。这种情况只会下载 dependencies 的依赖，其余依赖将会无视处理，这样别人下载你的 npm 包运行时，就会包含有 fs-extra jest 代码，从而增加本地项目的体积。
