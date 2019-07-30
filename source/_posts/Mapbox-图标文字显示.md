---
title: Mapbox 图标文字显示
date: 2019-07-30 16:41:34
tags:
  - mapbox
  - webGIS
categories: mapbox
---

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
