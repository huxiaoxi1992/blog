---
title: Mapbox 地图初始化
date: 2019-07-30 16:41:56
tags:
  - mapbox
  - webGIS
categories: mapbox
---

# Mapbox 地图初始化

本文主要针对于使用本地 GeoJSON 数据创建地图服务, 最终目的是使用本地 WebServer 启动静态地图页面, 如果需要使用 Mapbox 线上资源, 请参考 [Mapbox 线上示例](https://docs.mapbox.com/mapbox-gl-js/example/simple-map/)

完整示例[mapbox-sample.zip](https://www.yuque.com/attachments/yuque/0/2019/zip/431358/1564475099187-d53fd5e8-93af-4f5a-9aad-86f81fd76a4f.zip?_lake_card=%7B%22uid%22%3A%22rc-upload-1564475054960-4%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2019%2Fzip%2F431358%2F1564475099187-d53fd5e8-93af-4f5a-9aad-86f81fd76a4f.zip%22%2C%22name%22%3A%22mapbox-sample.zip%22%2C%22size%22%3A597753%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A0%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22CcZV6%22%2C%22card%22%3A%22file%22%7D)

<!-- more -->

<a name="VNUO4"></a>

## 创建 HTML 引入资源

打开编辑器, 创建 `index.html`, 并生成基础 HTML 结构.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Mapbox Map</title>
  </head>
  <body></body>
</html>
```

引入 `mapbox-gl.js` 和  `mapbox-gl.css` 文件, 由于官方的资源库连接速度较慢, 这里使用 `BootCDN` 的 CDN 服务.

```html
<script src="https://cdn.bootcss.com/mapbox-gl/1.1.0/mapbox-gl.js"></script>
<link
  href="https://cdn.bootcss.com/mapbox-gl/1.1.0/mapbox-gl.css"
  rel="stylesheet"
/>
```

由于本文目的在于使用本地 WebServer 启动地图页面, 需要将上述文件下载到本地, 与 index.html 放在相同目录下, 并修改 HTML 文件.

```html
<script src="./mapbox-gl.js"></script>
<link href="./mapbox-gl.css" rel="stylesheet" />
```

最终的 HTML 文件如下

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <script src="./mapbox-gl.js"></script>
    <link href="./mapbox-gl.css" rel="stylesheet" />
    <title>Mapbox Map</title>
  </head>
  <body></body>
</html>
```

<a name="Ahh1W"></a>

## 创建地图容器

在 HTML 文件 `body`  标签中间增加

```html
<div id="map"></div>
```

head 标签里增加 map 的样式表

```html
<style>
  body {
    margin: 0;
    padding: 0;
  }
  #map {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
  }
</style>
```

最终 HTML 文件如下

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <script src="./mapbox-gl.js"></script>
    <link href="./mapbox-gl.css" rel="stylesheet" />
    <title>Mapbox Map</title>
    <style>
      body {
        margin: 0;
        padding: 0;
      }
      #map {
        position: absolute;
        top: 0;
        bottom: 0;
        width: 100%;
      }
    </style>
  </head>
  <body>
    <div id="map"></div>
  </body>
</html>
```

<a name="Ketdw"></a>

## 地图初始化

当前目录创建 `index.js`, 并在 HTML 文件中引入

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <script src="./mapbox-gl.js"></script>
    <link href="./mapbox-gl.css" rel="stylesheet" />
    <title>Mapbox Map</title>
    <style>
      body {
        margin: 0;
        padding: 0;
      }
      #map {
        position: absolute;
        top: 0;
        bottom: 0;
        width: 100%;
      }
    </style>
  </head>
  <body>
    <div id="map"></div>
  </body>
  <script src="./index.js"></script>
</html>
```

在 `index.js` 文件中增加地图初始化定义

`index.js`

```javascript
const map = new mapboxgl.Map({
  container: "map", // 使用 id = map 的元素用于地图容器
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 无数据源使用
    sources: {},
    // 图层定义
    layers: [
      // 背景层
      {
        id: "background",
        type: "background",
        paint: {
          // 背景颜色
          "background-color": "#75cff0"
        }
      }
    ]
  }
});
```

使用浏览器打开 index.html, 此时显示为一片蓝色背景的地图

![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564468837023-2edea0a8-eac8-41c9-8727-77a868b27cbe.png#align=left&display=inline&height=1018&name=image.png&originHeight=1018&originWidth=1669&size=42920&status=done&width=1669)
