---
title: 瓦片地图与TMS服务
date: 2019-09-03 11:40:35
tags:
  - TMS
  - webGIS
categories: webGIS
---

# TMS 服务

TMS 是一种通用切片地图服务, 可直接通过 QGIS 与 webGIS 等方法加载底图

## QGIS 配置

> QGIS 版本需要大于等于 2.16

<!-- more -->

### 浏览器面板

![浏览器面板](./browser.png)

### 新建连接

右键 -> 新建连接 -> 复制下面常用的 TMS_URL

![新建连接](./url.png)

### 加载底图

添加图层至图层面板

![添加图层](./addLayer.png)

## 常用的 TMS_URL

1. 2gis_map: `http://tile2.maps.2gis.com/tiles?x={x}&y={y}&z={z}&v=1.1`
2. Bingmap: `http://ecn.dynamic.t0.tiles.virtualearth.net/comp/CompositionHandler/{q}?mkt=en-us&it=G,VE,BX,L,LA&shading=hill`
3. Bing_sat: `http://ecn.t3.tiles.virtualearth.net/tiles/a{q}.jpeg?g=0&dir=dir_n`
4. cartdb_positron: `http://a.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png`
5. ESRI_national_geographic: `http://services.arcgisonline.com/ArcGIS/rest/services/NatGeo_World_Map/MapServer/tile/{z}/{y}/{x}`
6. ESRI_sat: `https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}`
7. ESRI_standard: `https://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer/tile/{z}/{y}/{x}`
8. google_CN: `http://mt2.google.cn/vt/lyrs=m@177000000&hl=zh-CN&gl=cn&src=app&x={x}&y={y}&z={z}`
9. google_road: `https://mt1.google.com/vt/lyrs=m&x={x}&y={y}&z={z}`
10. google_sat_us: `https://mt1.google.com/vt/lyrs=s@110&x={x}&y={y}&z={z}`
11. google_sat: `http://mt3.google.cn/vt/lyrs=s@110&hl=zh-CN&gl=cn&src=app&x={x}&y={y}&z={z}`
12. landsat: `http://irs.gis-lab.info/?layers=landsat&request=GetTile&z={z}&x={x}&y={y}`
13. OSM: `http://c.tile.openstreetmap.org/{z}/{x}/{y}.png`
14. google_terrain: `http://mt0.google.com/vt/lyrs=t@130,r@203000000&hl=en&x={x}&y={y}&z={z}&s=Ga`
15. 高德地图: `http://webrd01.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}`

## 谷歌地址配置说明

http://mt0.google.com/vt/lyrs=m&hl=en&x={x}&y={y}&z={z}&s=Ga

```
h = roads only
m = standard roadmap
p = terrain
r = somehow altered roadmap
s = satellite only
t = terrain only
y = hybrid
```
