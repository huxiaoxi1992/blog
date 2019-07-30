---
title: Mapbox 地图教程
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

# Mapbox 地图使用本地 GeoJSON 数据

<a name="D5okl"></a>

## 下载地图资源

Mapbox 使用本地数据时, 需要引入

- 地图资源[maps.zip](https://www.yuque.com/attachments/yuque/0/2019/zip/431358/1564469498020-160bd0d6-2f4c-4d87-8a2a-158738bc92e1.zip?_lake_card=%7B%22uid%22%3A%22rc-upload-1564468876676-5%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2019%2Fzip%2F431358%2F1564469498020-160bd0d6-2f4c-4d87-8a2a-158738bc92e1.zip%22%2C%22name%22%3A%22maps.zip%22%2C%22size%22%3A129968%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A0%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22jMeRo%22%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2019%2Fzip%2F431358%2F1564469498020-160bd0d6-2f4c-4d87-8a2a-158738bc92e1.zip%22%2C%22card%22%3A%22file%22%7D).  这里使用大熊猫基地的地图数据作为示例.
- 英文字体包[font.zip](https://www.yuque.com/attachments/yuque/0/2019/zip/431358/1564469497985-61f9bf45-4ef4-4b71-970e-8dd615ed9c9d.zip?_lake_card=%7B%22uid%22%3A%22rc-upload-1564468876676-4%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2019%2Fzip%2F431358%2F1564469497985-61f9bf45-4ef4-4b71-970e-8dd615ed9c9d.zip%22%2C%22name%22%3A%22font.zip%22%2C%22size%22%3A76138%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A0%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22n8hUb%22%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2019%2Fzip%2F431358%2F1564469497985-61f9bf45-4ef4-4b71-970e-8dd615ed9c9d.zip%22%2C%22card%22%3A%22file%22%7D). 汉字字体可以使用页面字体加载, 英文字体必须通过 glyphs 加载.
- 地图雪碧图 [sprite.zip](https://www.yuque.com/attachments/yuque/0/2019/zip/431358/1564469498025-6d6ea5d4-7bfb-4f31-b0d3-f5f78d063015.zip?_lake_card=%7B%22uid%22%3A%22rc-upload-1564468876676-6%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2019%2Fzip%2F431358%2F1564469498025-6d6ea5d4-7bfb-4f31-b0d3-f5f78d063015.zip%22%2C%22name%22%3A%22sprite.zip%22%2C%22size%22%3A203991%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A0%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22QztIT%22%2C%22refSrc%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2019%2Fzip%2F431358%2F1564469498025-6d6ea5d4-7bfb-4f31-b0d3-f5f78d063015.zip%22%2C%22card%22%3A%22file%22%7D). 通常是一个项目使用一组雪碧图, 包含原始大小和 2x 的图片与 json 文件.

字体包与雪碧图的生成参考 [Mapbox 字体生成](https://www.yuque.com/huxiaoxi/ppwpsh/smz8xd)  与 [Mapbox 雪碧图制作](https://www.yuque.com/huxiaoxi/ppwpsh/mvclfa)  教程.

下载上述文件并解压到前文的根目录下, 此时的目录结构如下, font 目录下为形如 `0-255.pbf`  的多个 pbf 文件.

![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564469650662-1a2d992f-c88e-4013-a1d4-347e4f199860.png#align=left&display=inline&height=325&name=image.png&originHeight=325&originWidth=198&size=15246&status=done&width=198)

<!-- more -->

<a name="fuksw"></a>

## 下载安装 serve 用于启动本地 WebServer

需要安装 [nodejs](https://nodejs.org/zh-cn/).

```bash
$ npm i -g serve
```

在当前目录打开命令行, 输入

```bash
$ serve

# 显示结果如下
###   ┌───────────────────────────────────────────────┐
###   │                                               │
###   │   Serving!                                    │
###   │                                               │
###   │   - Local:            http://localhost:5000   │
###   │   - On Your Network:  http://10.0.75.1:5000   │
###   │                                               │
###   │   Copied local address to clipboard!          │
###   │                                               │
###   └───────────────────────────────────────────────┘
```

在本地启动了一个 WebServer, 监听  5000 端口, 默认打开当前目录下的 index.html 文件.

在浏览器中打开  [http://localhost:5000](http://localhost:5000), 可以看到与前文相同的页面

![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564471992521-cac164ef-fed0-4f75-ad78-9f5a0d8af8a7.png#align=left&display=inline&height=805&name=image.png&originHeight=805&originWidth=1097&size=24441&status=done&width=1097)

<a name="N37gS"></a>

## 增加地图数据源

修改 index.js 文件, 修改 sources 的值

```json
{
  "label": {
    "type": "geojson",
    "data": "./maps/label.geojson"
  },
  "polygon": {
    "type": "geojson",
    "data": "./maps/polygon.geojson"
  },
  "line": {
    "type": "geojson",
    "data": "./maps/line.geojson"
  }
}
```

修改后的 index.js

```javascript
const map = new mapboxgl.Map({
  container: "map", // 使用 id = map 的元素用于地图容器
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 使用 maps 目录下的 GeoJSON 文件作为数据源
    sources: {
      // 图标数据
      label: {
        type: "geojson",
        data: "./maps/label.geojson"
      },
      // 面数据
      polygon: {
        type: "geojson",
        data: "./maps/polygon.geojson"
      },
      // 线数据
      line: {
        type: "geojson",
        data: "./maps/line.geojson"
      }
    },
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

<a name="5p21S"></a>

## 增加地图样式

加载数据源后, 依然无法看到实际的地图显示, 这是因为还没有添加需要显示的面图层, 修改 layers

```json
[
  // 背景层
  {
    "id": "background",
    "type": "background",
    "paint": {
      // 背景颜色
      "background-color": "#75cff0"
    }
  },
  // 填充层
  {
    "id": "fill",
    "type": "fill",
    "source": "polygon",
    "filter": [
      "in",
      "type",
      "bottom",
      "grass",
      "green",
      "green_down",
      "green_middle",
      "river",
      "highway",
      "road"
    ],
    "paint": {
      // 填充颜色
      "fill-color": [
        // 按照 geojson type 属性进行过滤
        "match",
        ["get", "type"],
        "grass",
        "#d0e0c7",
        "green",
        "#9dcaaa",
        "green_middle",
        "#B5D7BC",
        "green_down",
        "#C5DFBC",
        "river",
        "#9ED3F3",
        "highway",
        "#F1F1F3",
        "bottom",
        "#ffffff",
        "road",
        "#FFFDF0",
        // 默认值
        "#000000"
      ],
      // 填充轮廓颜色
      "fill-outline-color": [
        "match",
        ["get", "type"],
        "grass",
        "#d0e0c7",
        "green",
        "#9dcaaa",
        "green_middle",
        "#B5D7BC",
        "green_down",
        "#C5DFBC",
        "river",
        "#9ED3F3",
        "highway",
        "#F1F1F3",
        "bottom",
        "#ffffff",
        "road",
        "#FFFDF0",
        "#000000"
      ]
    }
  }
]
```

刷新展示页面, 依然只有地图显示.

由于地图的默认中心设置在[0,0], 并未处于数据源的中心, 需要在 map 的参数中增加 zoom, minZoom, maxZoom, center 属性

```json
{
  //....

  // 当前地图显示等级
  "zoom": 16,
  // 地图最小缩放等级
  "minZoom": 15,
  // 地图最大缩放等级
  "maxZoom": 18,
  // 地图中心
  "center": [104.14417, 30.73596]

  //...
}
```

刷新页面, 展示结果如下, 此时可以使用鼠标拖动与缩放地图

![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564472657034-2fc2f814-a3ea-4b0d-885d-abd9b9770131.png#align=left&display=inline&height=956&name=image.png&originHeight=956&originWidth=1342&size=248047&status=done&width=1342)

此时的 index.js

```javascript
const map = new mapboxgl.Map({
  // 使用 id = map 的元素用于地图容器
  container: "map",
  // 当前地图显示等级
  zoom: 16,
  // 地图最小缩放等级
  minZoom: 15,
  // 地图最大缩放等级
  maxZoom: 18,
  // 地图中心
  center: [104.14417, 30.73596],
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 使用 maps 目录下的 GeoJSON 文件作为数据源
    sources: {
      // 图标数据
      label: {
        type: "geojson",
        data: "./maps/label.geojson"
      },
      // 面数据
      polygon: {
        type: "geojson",
        data: "./maps/polygon.geojson"
      },
      // 线数据
      line: {
        type: "geojson",
        data: "./maps/line.geojson"
      }
    },
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
      },
      // 填充层
      {
        id: "fill",
        type: "fill",
        source: "polygon",
        filter: [
          "in",
          "type",
          "bottom",
          "grass",
          "green",
          "green_down",
          "green_middle",
          "river",
          "highway",
          "road"
        ],
        paint: {
          // 填充颜色
          "fill-color": [
            // 按照 geojson type 属性进行过滤
            "match",
            ["get", "type"],
            "grass",
            "#d0e0c7",
            "green",
            "#9dcaaa",
            "green_middle",
            "#B5D7BC",
            "green_down",
            "#C5DFBC",
            "river",
            "#9ED3F3",
            "highway",
            "#F1F1F3",
            "bottom",
            "#ffffff",
            "road",
            "#FFFDF0",
            // 默认值
            "#000000"
          ],
          // 填充轮廓颜色
          "fill-outline-color": [
            "match",
            ["get", "type"],
            "grass",
            "#d0e0c7",
            "green",
            "#9dcaaa",
            "green_middle",
            "#B5D7BC",
            "green_down",
            "#C5DFBC",
            "river",
            "#9ED3F3",
            "highway",
            "#F1F1F3",
            "bottom",
            "#ffffff",
            "road",
            "#FFFDF0",
            "#000000"
          ]
        }
      }
    ]
  }
});
```

# Mapbox 图标文字显示

<a name="RD8vI"></a>

## 增加图标与文字

layers 增加道路文字图层与图标文字层

```json
// 道路文字
({
  "id": "roadLabel",
  "type": "symbol",
  // 使用 line 的数据源
  "source": "line",
  // 过滤 type 进行选择
  "filter": ["in", "type", "highway", "motorway"],
  "layout": {
    "symbol-spacing": 350,
    "symbol-placement": "line",
    "text-field": "{label}",
    "text-font": ["font"],
    // 按照 type 字段定义字体大小, 默认值为 18
    "text-size": ["match", ["get", "type"], "highway", 16, 18]
  },
  "paint": {
    "text-color": "#415A59",
    "text-halo-color": "#fff",
    "text-halo-width": 0.5
  }
},
// 图标与文字
{
  "id": "label",
  "type": "symbol",
  // 使用 label 作为数据源
  "source": "label",
  "layout": {
    // 使用 type 字段对应的雪碧图进行选择
    "icon-image": "{type}",
    "icon-size": 0.3,
    // 使用 label 字段进行文字填充
    "text-field": "{label}",
    // 使用目录下 font/ 文件夹下的字体
    "text-font": ["font"],
    "text-size": 12,
    "text-offset": [0, 1],
    "text-anchor": "top"
  },
  "paint": {
    "text-color": "#415A59",
    "text-halo-color": "#fff",
    "text-halo-width": 0.5
  }
})
```

<!-- more -->

此时刷新页面, 会提示出错<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564474414825-175e6556-5454-4793-aa68-bd29b55360da.png#align=left&display=inline&height=184&name=image.png&originHeight=184&originWidth=560&size=22705&status=done&width=560)

<a name="0OwUV"></a>

## 增加  glyphs 与  sprite 路径

需要指定样式使用的字体包路径, 在 style 中添加属性  glyphs

```json
{
  // 字体包路径
  "glyphs": "./{fontstack}/{range}.pbf"
}
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564474489428-cf614528-aa6a-4956-bd62-9bf5e222d4c0.png#align=left&display=inline&height=1014&name=image.png&originHeight=1014&originWidth=1601&size=421014&status=done&width=1601)<br />可以看到文字已经加入地图上, 但图标还未显示成功, 使用相同的方法, 在 style 中增加属性  sprite

```json
{
  // 雪碧图路径
  "sprite": "./sprite/sprite"
}
```

<a name="CrnCs"></a>

## 解决图标路径 BUG

刷新页面, 无法加载地图, 报错   `Unable to parse URL object`<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564474650796-f3a4f188-06f1-4724-8cf7-84653246d200.png#align=left&display=inline&height=108&name=image.png&originHeight=108&originWidth=556&size=12333&status=done&width=556)

Mapbox 无法解析相对路径下的 sprite 路径, 需要修改 sprite 属性, 使用当前页面路径进行填充.

```javascript
// 当前页面绝对路径, sprite 无法使用相对路径获取, 因此定义 locationURL
const locationURL =
  window.location.origin +
  window.location.pathname.substring(
    0,
    window.location.pathname.lastIndexOf("/")
  );

//...
	style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 雪碧图路径
    sprite: `${locationURL}/sprite/sprite`,
    // 字体包路径
    glyphs: "./{fontstack}/{range}.pbf",
//...
  }
```

此时的地图展示如下<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/431358/1564474853992-0105e276-e295-49be-aab9-57c4c0a97ff6.png#align=left&display=inline&height=906&name=image.png&originHeight=906&originWidth=1319&size=305802&status=done&width=1319)

完整 index.js 文件如下

```javascript
// 当前页面绝对路径, sprite 无法使用相对路径获取, 因此定义 locationURL
const locationURL =
  window.location.origin +
  window.location.pathname.substring(
    0,
    window.location.pathname.lastIndexOf("/")
  );

const map = new mapboxgl.Map({
  container: "map", // 使用 id = map 的元素用于地图容器
  // 当前地图显示等级
  zoom: 16,
  // 地图最小缩放等级
  minZoom: 15,
  // 地图最大缩放等级
  maxZoom: 18,
  // 地图中心
  center: [104.14417, 30.73596],
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 雪碧图路径
    sprite: `${locationURL}/sprite/sprite`,
    // 字体包路径
    glyphs: "./{fontstack}/{range}.pbf",
    // 使用 maps 目录下的 GeoJSON 文件作为数据源
    sources: {
      // 图标数据
      label: {
        type: "geojson",
        data: "./maps/label.geojson"
      },
      // 面数据
      polygon: {
        type: "geojson",
        data: "./maps/polygon.geojson"
      },
      // 线数据
      line: {
        type: "geojson",
        data: "./maps/line.geojson"
      }
    },
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
      },
      // 填充层
      {
        id: "fill",
        type: "fill",
        source: "polygon",
        filter: [
          "in",
          "type",
          "bottom",
          "grass",
          "green",
          "green_down",
          "green_middle",
          "river",
          "highway",
          "road"
        ],
        paint: {
          // 填充颜色
          "fill-color": [
            // 按照 geojson type 属性进行过滤
            "match",
            ["get", "type"],
            "grass",
            "#d0e0c7",
            "green",
            "#9dcaaa",
            "green_middle",
            "#B5D7BC",
            "green_down",
            "#C5DFBC",
            "river",
            "#9ED3F3",
            "highway",
            "#F1F1F3",
            "bottom",
            "#ffffff",
            "road",
            "#FFFDF0",
            // 默认值
            "#000000"
          ],
          // 填充轮廓颜色
          "fill-outline-color": [
            "match",
            ["get", "type"],
            "grass",
            "#d0e0c7",
            "green",
            "#9dcaaa",
            "green_middle",
            "#B5D7BC",
            "green_down",
            "#C5DFBC",
            "river",
            "#9ED3F3",
            "highway",
            "#F1F1F3",
            "bottom",
            "#ffffff",
            "road",
            "#FFFDF0",
            "#000000"
          ]
        }
      },
      // 道路文字
      {
        id: "roadLabel",
        type: "symbol",
        source: "line",
        filter: ["in", "type", "highway", "motorway"],
        layout: {
          "symbol-spacing": 350,
          "symbol-placement": "line",
          "text-field": "{label}",
          "text-font": ["font"],
          "text-size": ["match", ["get", "type"], "highway", 16, 18]
        },
        paint: {
          "text-color": "#415A59",
          "text-halo-color": "#fff",
          "text-halo-width": 0.5
        }
      },
      // 图标与文字
      {
        id: "label",
        type: "symbol",
        source: "label",
        layout: {
          "icon-image": "{type}",
          "icon-size": 0.3,
          "text-field": "{label}",
          "text-font": ["font"],
          "text-size": 12,
          "text-offset": [0, 1],
          "text-anchor": "top"
        },
        paint: {
          "text-color": "#415A59",
          "text-halo-color": "#fff",
          "text-halo-width": 0.5
        }
      }
    ]
  }
});
```
