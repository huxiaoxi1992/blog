---
title: Mapbox 地图教程
date: 2019-07-30 16:41:56
tags:
  - mapbox
  - webGIS
categories: mapbox
---

# Mapbox 地图使用教程

## Mapbox 地图初始化

本文主要针对于使用本地 GeoJSON 数据创建地图服务, 最终目的是使用本地 WebServer 启动静态地图页面, 如果需要使用 Mapbox 线上资源, 请参考 [Mapbox 线上示例](https://docs.mapbox.com/mapbox-gl-js/example/simple-map/)

<!-- more -->

完整示例 [mapbox-sample.zip](./mapbox-sample.zip)

### 创建 HTML 引入资源

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

引入 `mapbox-gl.js` 和 `mapbox-gl.css` 文件, 由于官方的资源库连接速度较慢, 这里使用 `BootCDN` 的 CDN 服务.

```diff
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
+    <script src="https://cdn.bootcss.com/mapbox-gl/1.1.0/mapbox-gl.js"></script>
+    <link href="https://cdn.bootcss.com/mapbox-gl/1.1.0/mapbox-gl.css" rel="stylesheet" />
  </head>
  <body></body>
</html>
```

由于本文目的在于使用本地 WebServer 启动地图页面, 需要将上述文件下载到本地, 与 index.html 放在相同目录下, 并修改 HTML 文件.

```diff
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
-   <script src="https://cdn.bootcss.com/mapbox-gl/1.1.0/mapbox-gl.js"></script>
-   <link href="https://cdn.bootcss.com/mapbox-gl/1.1.0/mapbox-gl.css" rel="stylesheet" />
+   <script src="./mapbox-gl.js"></script>
+   <link href="./mapbox-gl.css" rel="stylesheet" />
  </head>
  <body></body>
</html>
```

### 创建地图容器

在 HTML 文件 `body` 标签中间增加

```diff
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
    <script src="./mapbox-gl.js"></script>
    <link href="./mapbox-gl.css" rel="stylesheet" />
  </head>
  <body>
+    <div id="map"></div>
  </body>
</html>
```

head 标签里增加 map 的样式表

```diff
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
    <script src="./mapbox-gl.js"></script>
    <link href="./mapbox-gl.css" rel="stylesheet" />
+    <style>
+      body {
+        margin: 0;
+        padding: 0;
+      }
+      #map {
+        position: absolute;
+        top: 0;
+        bottom: 0;
+        width: 100%;
+      }
+    </style>
  </head>
  <body>
    <div id="map"></div>
  </body>
</html>
```

### 地图初始化

当前目录创建 `index.js`, 并在 HTML 文件中引入

```diff
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
    <script src="./mapbox-gl.js"></script>
    <link href="./mapbox-gl.css" rel="stylesheet" />
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
+  <script src="./index.js"></script>
</html>
```

在 `index.js` 文件中增加地图初始化定义

```js
// index.js

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
          "background-color": "#75cff0",
        },
      },
    ],
  },
});
```

使用浏览器打开 index.html, 此时显示为一片蓝色背景的地图

![image.png](./1564468837023-2edea0a8-eac8-41c9-8727-77a868b27cbe.png)

## 使用本地 GeoJSON 数据

### 下载地图资源

Mapbox 使用本地数据时, 需要引入

- 地图资源 [maps.zip](./maps.zip). 这里使用大熊猫基地的地图数据作为示例.
- 英文字体包 [font.zip](./font.zip). 汉字字体可以使用页面字体加载, 英文字体必须通过 glyphs 加载.
- 地图雪碧图 [sprite.zip](./sprite.zip). 通常是一个项目使用一组雪碧图, 包含原始大小和 2x 的图片与 json 文件.

雪碧图的生成参考 [Mapbox 雪碧图制作](/2019/07/23/雪碧图制作/) 教程.

下载上述文件并解压到前文的根目录下, 此时的目录结构如下, font 目录下为形如 `0-255.pbf` 的多个 pbf 文件.

![image.png](./1564469650662-1a2d992f-c88e-4013-a1d4-347e4f199860.png)

### 下载安装 serve 用于启动本地 WebServer

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

在本地启动了一个 WebServer, 监听 5000 端口, 默认打开当前目录下的 index.html 文件.

在浏览器中打开 [http://localhost:5000](http://localhost:5000), 可以看到与前文相同的页面

![image.png](./1564471992521-cac164ef-fed0-4f75-ad78-9f5a0d8af8a7.png)

### 增加地图数据源

修改 index.js 文件, 修改 sources 的值

```diff
// index.js

const map = new mapboxgl.Map({
  container: "map", // 使用 id = map 的元素用于地图容器
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 使用 maps 目录下的 GeoJSON 文件作为数据源
    sources: {
+      // 图标数据
+      label: {
+        type: "geojson",
+        data: "./maps/label.geojson"
+      },
+      // 面数据
+      polygon: {
+        type: "geojson",
+        data: "./maps/polygon.geojson"
+      },
+      // 线数据
+      line: {
+        type: "geojson",
+        data: "./maps/line.geojson"
+      }
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

### 增加地图样式

加载数据源后, 依然无法看到实际的地图显示, 这是因为还没有添加需要显示的面图层, 增加 layers 中图层

```diff
// index.js

const map = new mapboxgl.Map({
  // 使用 id = map 的元素用于地图容器
  container: "map",
  // 样式
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
+      // 填充层
+      {
+        id: "fill",
+        type: "fill",
+        source: "polygon",
+        filter: [
+          "in",
+          "type",
+          "bottom",
+          "grass",
+          "green",
+          "green_down",
+          "green_middle",
+          "river",
+          "highway",
+          "road"
+        ],
+        paint: {
+          // 填充颜色
+          "fill-color": [
+            // 按照 geojson type 属性进行过滤
+            "match",
+            ["get", "type"],
+            "grass",
+            "#d0e0c7",
+            "green",
+            "#9dcaaa",
+            "green_middle",
+            "#B5D7BC",
+            "green_down",
+            "#C5DFBC",
+            "river",
+            "#9ED3F3",
+            "highway",
+            "#F1F1F3",
+            "bottom",
+            "#ffffff",
+            "road",
+            "#FFFDF0",
+            // 默认值
+            "#000000"
+          ],
+          // 填充轮廓颜色
+          "fill-outline-color": [
+            "match",
+            ["get", "type"],
+            "grass",
+            "#d0e0c7",
+            "green",
+            "#9dcaaa",
+            "green_middle",
+            "#B5D7BC",
+            "green_down",
+            "#C5DFBC",
+            "river",
+            "#9ED3F3",
+            "highway",
+            "#F1F1F3",
+            "bottom",
+            "#ffffff",
+            "road",
+            "#FFFDF0",
+            "#000000"
+          ]
+        }
+      }
    ]
  }
});
```

刷新展示页面, 依然只有地图显示.

由于地图的默认中心设置在 [0,0], 并未处于数据源的中心, 需要在 `map` 的参数中增加 `zoom`, `minZoom`, `maxZoom`, `center` 属性

```diff
// index.js

const map = new mapboxgl.Map({
  container: "map", // 使用 id = map 的元素用于地图容器
+  // 当前地图显示等级
+  zoom: 16,
+  // 地图最小缩放等级
+  minZoom: 15,
+  // 地图最大缩放等级
+  maxZoom: 18,
+  // 地图中心
+  center: [104.14417, 30.73596]
  // 样式
  style:{
  //.....
  }
});
```

刷新页面, 展示结果如下, 此时可以使用鼠标拖动与缩放地图

![image.png](./1564472657034-2fc2f814-a3ea-4b0d-885d-abd9b9770131.png)

此时刷新页面, 会提示出错

![image.png](./1564474414825-175e6556-5454-4793-aa68-bd29b55360da.png)

### 增加 glyphs 与 sprite 路径

需要指定样式使用的字体包路径, 在 style 中添加属性 glyphs

```diff
// index.js

const map = new mapboxgl.Map({
  //.......

  // 样式
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
+    // 字体包路径
+    glyphs: "./{fontstack}/{range}.pbf"
    //......
  }
});
```

![image.png](./1564474489428-cf614528-aa6a-4956-bd62-9bf5e222d4c0.png)

可以看到文字已经加入地图上, 但图标还未显示成功, 使用相同的方法, 在 style 中增加属性 sprite

```diff
// index.js

const map = new mapboxgl.Map({
  //.......

  // 样式
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 字体包路径
    glyphs: "./{fontstack}/{range}.pbf",
+    // 雪碧图路径
+    sprite: "./sprite/sprite"
    //......
  }
});
```

### 解决图标路径 BUG

刷新页面, 无法加载地图, 报错 `Unable to parse URL object`

![image.png](./1564474650796-f3a4f188-06f1-4724-8cf7-84653246d200.png)

Mapbox 无法解析相对路径下的 sprite 路径, 需要修改 sprite 属性, 使用当前页面路径进行填充.

```diff
// index.js

+ // 当前页面绝对路径, sprite 无法使用相对路径获取, 因此定义 locationURL
+ const locationURL =
+   window.location.origin +
+   window.location.pathname.substring(
+     0,
+     window.location.pathname.lastIndexOf("/")
+   );

const map = new mapboxgl.Map({
  //......

  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 字体包路径
    glyphs: "./{fontstack}/{range}.pbf",
    // 雪碧图路径
-   sprite: "./sprite/sprite"
+   sprite: `${locationURL}/sprite/sprite`
    //...
  }
});
```

此时的地图展示如下

![image.png](./1564474853992-0105e276-e295-49be-aab9-57c4c0a97ff6.png)

完整 index.js 文件如下

```js
// index.js

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
  // 样式
  style: {
    // 样式版本, 本地加载使用 version: 8
    version: 8,
    // 字体包路径
    glyphs: "./{fontstack}/{range}.pbf",
    // 雪碧图路径
    sprite: `${locationURL}/sprite/sprite`,
    // 使用 maps 目录下的 GeoJSON 文件作为数据源
    sources: {
      // 图标数据
      label: {
        type: "geojson",
        data: "./maps/label.geojson",
      },
      // 面数据
      polygon: {
        type: "geojson",
        data: "./maps/polygon.geojson",
      },
      // 线数据
      line: {
        type: "geojson",
        data: "./maps/line.geojson",
      },
    },
    // 图层定义
    layers: [
      // 背景层
      {
        id: "background",
        type: "background",
        paint: {
          // 背景颜色
          "background-color": "#75cff0",
        },
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
          "road",
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
            "#000000",
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
            "#000000",
          ],
        },
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
          "text-size": ["match", ["get", "type"], "highway", 16, 18],
        },
        paint: {
          "text-color": "#415A59",
          "text-halo-color": "#fff",
          "text-halo-width": 0.5,
        },
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
          "text-anchor": "top",
        },
        paint: {
          "text-color": "#415A59",
          "text-halo-color": "#fff",
          "text-halo-width": 0.5,
        },
      },
    ],
  },
});
```
