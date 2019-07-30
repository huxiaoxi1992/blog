---
title: Mapbox 地图使用本地 GeoJSON 数据
date: 2019-07-30 16:42:07
tags:
  - mapbox
  - webGIS
categories: mapbox
---

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
