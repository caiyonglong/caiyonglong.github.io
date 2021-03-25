---
title: 爱壁纸音乐api
tags:
  - api
categories: 接口
description: 壁纸接口
toc: true 文章目录
abbrlink: f296039d
date: 2016-08-25 08:21:16
author:
comments:
original:
permalink:
---
# 前言
通过fildder抓取爱壁纸app的图片接口

```
limit:10  //壁纸数量,
order:hot //排序方式
skip:1 //偏移量

//参数集合
var map: Map<String, Any> = mapOf("limit" to limit,
                "skip" to skip,
                "order" to order,
                "adult" to "false",
                "first" to "0")
```

主页获取壁纸图片 GET方式获取
http://service.aibizhi.adesk.com/v3/homepage 

例如：
http://service.aibizhi.adesk.com/v3/homepage?limit=30&adult=false&did=867919026491418&first=1&order=hot


最新
http://service.aibizhi.adesk.com/v1/vertical/vertical?limit=30&adult=false&first=1&order=new


album
http://service.aibizhi.adesk.com/v1/wallpaper/album/50b2e4de0a2ae035e5d7139b/wallpaper?limit=30&adult=false&first=1&order=new

热门
http://service.aibizhi.adesk.com/v3/wallpaper?limit=30&adult=false&first=1&order=hot

分类
http://service.aibizhi.adesk.com/v1/wallpaper/category?adult=false&first=1


http://service.aibizhi.adesk.com/v1/vertical/category/4e4d610cdf714d2966000000/vertical?limit=10&adult=false&first=1&order=new


返回的数据类型如下
```
{
    "msg": "success",
    "res": {
        "ads": [],
        "vertical": [
            {
                "views": 0,
                "ncos": 0,
                "rank": 6,
                "tag": [],
                "wp": "http://img.aibizhi.adesk.com/5acb3d44e7bce73542df002d",
                "xr": false,
                "cr": false,
                "favs": 6,
                "atime": 1524575403,
                "id": "5acb3d44e7bce73542df002d",
                "desc": "",
                "thumb": "http://img.aibizhi.adesk.com/5acb3d44e7bce73542df002d?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540",
                "img": "http://img.aibizhi.adesk.com/5acb3d44e7bce73542df002d?imageMogr2/thumbnail/!720x1280r/gravity/Center/crop/720x1280",
                "cid": [
                    "4e4d610cdf714d2966000002"
                ],
                "url": [],
                "rule": "?imageMogr2/thumbnail/!$<Width>x$<Height>r/gravity/Center/crop/$<Width>x$<Height>",
                "preview": "http://img.aibizhi.adesk.com/5acb3d44e7bce73542df002d",
                "store": "qiniu"
            }
        ]
    },
    "code": 0
}
```

具体实现效果见[WallpaperAPP](https://github.com/caiyonglong/WallPaperApp)