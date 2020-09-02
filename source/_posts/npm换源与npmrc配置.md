---
title: npm换源与npmrc配置
date: 2020-03-30 18:12:41
tags:
  - npm
  - 前端开发
categories: web前端
---

## npm 换源

### 修改源地址为淘宝 NPM 镜像

```sh
npm config set registry http://registry.npm.taobao.org/
```

### 修改源地址为官方源

```sh
npm config set registry https://registry.npmjs.org/
```

在设置 registry 后运行 npm i -g mirror-config-china 安装镜像配置

## .npmrc 配置

可以通过修改 `~/.npmrc` 文件指定 npm 源与相关包的镜像, 常用配置如下

```sh
# npm 源
registry=https://registry.npm.taobao.org/
# electron 镜像源
electron_mirror=https://npm.taobao.org/mirrors/electron/
# sass 镜像源
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
# puppeteer 镜像源
puppeteer_download_host=https://npm.taobao.org/mirrors
```
