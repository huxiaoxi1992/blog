---
title: GeoJSON格式说明
date: 2021-04-08 14:54:26
tags:
  - GIS
categories: GIS
---

## 简介

GeoJSON 是一种对各种地理数据结构进行编码的格式。GeoJSON 对象可以表示几何、特征或者特征集合。GeoJSON 支持下面几何类型：点、线、面、多点、多线、多面和几何集合。GeoJSON 里的特征包含一个几何对象和其他属性，特征集合表示一系列特征。

<!-- more -->

一个完整的 GeoJSON 数据结构总是一个（JSON 术语里的）对象。在 GeoJSON 里，对象由名/值对--也称作成员的集合组成。对每个成员来说，名字总是字符串。成员的值要么是字符串、数字、对象、数组，要么是下面文本常量中的一个："true","false"和"null"。数组是由值是上面所说的元素组成。

### 举例

GeoJSON 特征集合：

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [102.0, 0.5] },
      "properties": { "prop0": "value0" }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [102.0, 0.0],
          [103.0, 1.0],
          [104.0, 0.0],
          [105.0, 1.0]
        ]
      },
      "properties": {
        "prop0": "value0",
        "prop1": 0.0
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [100.0, 0.0],
            [101.0, 0.0],
            [101.0, 1.0],
            [100.0, 1.0],
            [100.0, 0.0]
          ]
        ]
      },
      "properties": {
        "prop0": "value0",
        "prop1": { "this": "that" }
      }
    }
  ]
}
```

### 定义

- JavaScript 对象表示和术语对象、名字、值、数组和数字在 IETF RFC 4627 即http://www.ietf.org/rfc/rfc4627.txt里定义。

- 这篇文档里的关键字“必须“，”不允许“，”需要“，”应当“，”应当不“，”应该“，”不应该“，”推荐的“，”也许“和”可选的“在 IETF RFC 2119， 即http://www.ietf.org/rfc/rfc2119.txt里解释。

## GeoJSON 对象

GeoJSON 总是由一个单独的对象组成。这个对象（指的是下面的 GeoJSON 对象）表示几何、特征或者特征集合。

- GeoJSON 对象可能有任何数目成员（名/值对）。
- GeoJSON 对象必须由一个名字为"type"的成员。这个成员的值是由 GeoJSON 对象的类型所确定的字符串。
- type 成员的值必须是下面之一："Point", "MultiPoint", "LineString", "MultiLineString", "Polygon",    "MultiPolygon",   "GeometryCollection", "Feature", 或者 "FeatureCollection"。这儿 type 成员值必须如这儿所示。
- GeoJSON 对象可能有一个可选的"crs"成员，它的值必须是一个坐标参考系统的对象（见 3.坐标参考系统对象）。
- GeoJSON 对象可能有一个"bbox"成员，它的值必须是边界框数组（见 4.边界框）。

### 几何对象

几何是一种 GeoJSON 对象，这时 type 成员的值是下面字符串之一："Point", "MultiPoint", "LineString", "MultiLineString",  "Polygon", "MultiPolygon", 或者"GeometryCollection"。

除了“GeometryCollection”外的其他任何类型的 GeoJSON 几何对象必须由一个名字为"coordinates"的成员。coordinates 成员的值总是数组。这个数组里的元素的结构由几何类型来确定。

#### 位置

位置是基本的几何结构。几何对象的"coordinates"成员由一个位置（这儿是几何点）、位置数组（线或者几何多点），位置数组的数组（面、多线）或者位置的多维数组（多面）组成。

位置由数字数组表示。必须至少两个元素，可以有更多元素。元素的顺序必须遵从 x,y,z 顺序（投影坐标参考系统中坐标的东向、北向、高度或者地理坐标参考系统中的坐标长度、纬度、高度）。任何数目的其他元素是允许的---其他元素的说明和意义超出了这篇规格说明的范围。

位置和几何的例子在附录 A.几何例子里呈现。

#### 点

对类型"Point"来说，“coordinates"成员必须是一个单独的位置。

#### 多点

对类型"MultiPoint"来说，"coordinates"成员必须是位置数组。

#### 线

对类型"LineString"来说，“coordinates"成员必须是两个或者多个位置的数组。

线性环市具有 4 个或者更多位置的封闭的线。第一个和最后一个位置是相等的（它们表示相同的的点）。虽然线性环没有鲜明地作为 GeoJSON 几何类型，不过在面几何类型定义里有提到它。

#### 多线

对类型“MultiLineString"来说，"coordinates"成员必须是一个线坐标数组的数组。

#### 面

对类型"Polygon"来说，"coordinates"成员必须是一个线性环坐标数组的数组。对拥有多个环的的面来说，第一个环必须是外部环，其他的必须是内部环或者孔。

#### 多面

对类型"MultiPolygon"来说，"coordinates"成员必须是面坐标数组的数组。

#### 几何集合

类型为"GeometryCollection"的 GeoJSON 对象是一个集合对象，它表示几何对象的集合。

几何集合必须有一个名字为"geometries"的成员。与"geometries"相对应的值是一个数组。这个数组中的每个元素都是一个 GeoJSON 几何对象。

### 特征对象  

类型为"Feature"的 GeoJSON 对象是特征对象。

- 特征对象必须由一个名字为"geometry"的成员，这个几何成员的值是上面定义的几何对象或者 JSON 的 null 值。

- 特征对戏那个必须有一个名字为“properties"的成员，这个属性成员的值是一个对象（任何 JSON 对象或者 JSON 的 null 值）。

- 如果特征是常用的标识符，那么这个标识符应当包含名字为“id”的特征对象成员。

#### 特征集合对象

类型为"FeatureCollection"的 GeoJSON 对象是特征集合对象。

类型为"FeatureCollection"的对象必须由一个名字为"features"的成员。与“features"相对应的值是一个数组。这个数组中的每个元素都是上面定义的特征对象。

## 坐标参考系统对象

GeoJSON 对象的坐标参考系统（CRS）是由它的"crs"成员（指的是下面的 CRS 对象）来确定的。如果对象没有 crs 成员，那么它的父对象或者祖父对象的 crs 成员可能被获取作为它的 crs。如果这样还没有获得 crs 成员，那么默认的 CRS 将应用到 GeoJSON 对象。

- 默认的 CRS 是地理坐标参考系统，使用的是 WGS84 数据，长度和高度的单位是十进制标示。
- 名字为"crs"成员的值必须是 JSON 对象（指的是下面的 CRS 对象）或者 JSON 的 null。如果 CRS 的值为 null,那么就假设没有 CRS 了。
- crs 成员应当位于（特征集合、特征、几何的顺序的）层级结构里 GeoJSON 对象的最顶级，而且在自对象或者孙子对象里不应该重复或者覆盖。
- 非空的 CRS 对象有两个强制拥有的对象:"type"和"properties"。
- type 成员的值必须是字符串，这个字符串说明了 CRS 对象的类型。
- 属性成员的值必须是对象。
- CRS 应不能更改坐标顺序。

### 名字 CRS

CRS 对象可以通过名字来表明坐标参考系统。在这种情况下，它的"type"成员的值必须是字符串"name"。它的"properties"成员的值必须是包含"name"成员的对象。这个"name"成员的值必须是标识坐标参考系统的字符串。比如“urn:ogc:def:crs:OGC:1.3:CRS84"的 OGC CRS 的 URN 应当优先于旧的标识符如"EPSG:4326"得到选用：

```json
"crs": {
  "type": "name",
  "properties": {
    "name": "urn:ogc:def:crs:OGC:1.3:CRS84"
    }
  }
```

### 连接 CRS

CRS 对象也可以连接到互联网上的 CRS 参数。在这种情况下，它的"type"成员的值必须是字符串"link",它的"properties"成员的值必须是一个连接对象（见 3.2.1.连接对象） 。

#### 连接对象

连接对象由一个必需的成员："href"，和一个可选的成员:"type"。

必需的"href"成员的值必须是解引用的 URI（统一资源标识）。

可选的"type"成员的值必须是字符串，而且这个字符串暗示了所提供的 URI 里用来表示 CRS 参数的格式。建议值是:"proj4","ogcwkt",esriwkt",不过可以使用其他值：

```json
"crs": {
  "type": "link",
  "properties": {
    "href": "http://example.com/crs/42",
    "type": "proj4"
    }
  }
```

相对连接常常可以作为辅助文件里的 CRS 的直接处理器：

```json
"crs": {
  "type": "link",
  "properties": {
    "href": "data.crs",
    "type": "ogcwkt"
    }
  }
```

## 边界框

为了包含几何、特征或者特征集合的坐标范围信息，GeoJSON 对象可能有一个名字为"bbox 的成员。bbox 成员的值必须是 2\*n 数组，这儿 n 是所包含几何对象的维数，并且所有坐标轴的最低值后面跟着最高者值。bbox 的坐标轴的顺序遵循几何坐标轴的顺序。除此之外，bbox 的坐标参考系统假设匹配它所在 GeoJSON 对象的坐标参考系统。

特征对象上的 bbox 成员的例子：

```json
{
  "type": "Feature",
  "bbox": [-180.0, -90.0, 180.0, 90.0],
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [-180.0, 10.0],
        [20.0, 90.0],
        [180.0, -5.0],
        [-30.0, -90.0]
      ]
    ]
  }
}
```

特征集合对象 bbox 成员的例子：

```json
{
  "type": "FeatureCollection",
  "bbox": [100.0, 0.0, 105.0, 1.0],
  "features": [
    //...
  ]
}
```

## 附录 A.集合例子

下面例子中的每一个都表示一个完整的 GeoJSON 对象。注意 JSON 对象里的结尾的空白字符没有意义。例子里所用的空白符有助于说明这个数据结构，不过不是必需的。

### 点

点坐标是按照 x,y 顺序的(投影坐标的东向、北向，地理坐标的长度、高度）：

```json
{ "type": "Point", "coordinates": [100.0, 0.0] }
```

### 线

线的坐标是位置数组（见 2.1.1.位置）：

```json
{
  "type": "LineString",
  "coordinates": [
    [100.0, 0.0],
    [101.0, 1.0]
  ]
}
```

### 面

面的坐标是线性环坐标数组的数组。这个数组的第一个元素表示的是外部环。其他后续的元素表示的内部环（或者孔）。

没有孔的：

```json
{
  "type": "Polygon",
  "coordinates": [
    [
      [100.0, 0.0],
      [101.0, 0.0],
      [101.0, 1.0],
      [100.0, 1.0],
      [100.0, 0.0]
    ]
  ]
}
```

有孔的：

```json
{
  "type": "Polygon",
  "coordinates": [
    [
      [100.0, 0.0],
      [101.0, 0.0],
      [101.0, 1.0],
      [100.0, 1.0],
      [100.0, 0.0]
    ],
    [
      [100.2, 0.2],
      [100.8, 0.2],
      [100.8, 0.8],
      [100.2, 0.8],
      [100.2, 0.2]
    ]
  ]
}
```

### 多点

多点的坐标是位置数组：

```json
{
  "type": "MultiPoint",
  "coordinates": [
    [100.0, 0.0],
    [101.0, 1.0]
  ]
}
```

### 多线

多线的坐标是线坐标数组的数组:

```json
{
  "type": "MultiLineString",
  "coordinates": [
    [
      [100.0, 0.0],
      [101.0, 1.0]
    ],
    [
      [102.0, 2.0],
      [103.0, 3.0]
    ]
  ]
}
```

### 多面

多面的坐标是面坐标数组的数组:

```json
{
  "type": "MultiPolygon",
  "coordinates": [
    [
      [
        [102.0, 2.0],
        [103.0, 2.0],
        [103.0, 3.0],
        [102.0, 3.0],
        [102.0, 2.0]
      ]
    ],
    [
      [
        [100.0, 0.0],
        [101.0, 0.0],
        [101.0, 1.0],
        [100.0, 1.0],
        [100.0, 0.0]
      ],
      [
        [100.2, 0.2],
        [100.8, 0.2],
        [100.8, 0.8],
        [100.2, 0.8],
        [100.2, 0.2]
      ]
    ]
  ]
}
```

### 几何集合

几何集合的几何数组里的每个元素都是上面所描述的几何对象之一:

```json
{
  "type": "GeometryCollection",
  "geometries": [
    { "type": "Point", "coordinates": [100.0, 0.0] },
    {
      "type": "LineString",
      "coordinates": [
        [101.0, 0.0],
        [102.0, 1.0]
      ]
    }
  ]
}
```
