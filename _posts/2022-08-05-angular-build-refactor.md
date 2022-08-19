---
title: Angular build refactor
author: beni
date: Fri Aug 05 2022 13:51:23 GMT+0800
categories: [Blogging, FrontEnd]
tags: [CLI，打包，前端, Angular]
mermaid: true
# image:
#   path: /commons/devices-mockup.png
#   width: 800
#   height: 500
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

项目中Angular升级以来，一直没有对打包做过深度得优化处理，基本得目标能跑通即可。本地修改主要优化打包速度，主要是
移除那些deadcode，重复得code，提升打包性能，并且支持npm高版本install，保证依赖检查可以正常工作，不必使用(--legacy-peer-deps), 保证npm和node得更新适配;
通过打包优化，对打包和包版本增加更深入得了解

# Smartcmp angular 打包优化
---
---

## package.json/package-lock.json 包优化

```json
```

## 去除遗留require.js



## 打包体积优化


## todo, 由难易程度罗列剩下打包优化List，为整体打包做准备


## 线上打包完整时间分析

* triggered 15：38
* git load remote repository 15:38 - 15:42
* bash package.sh 
    - 15:43 - 15:58  install packages
    - 15:59 - 16:16  build package end
    - 16:16 - 16:36  SonarScanner
* total 1h




