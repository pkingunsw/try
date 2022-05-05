---
title: R语言数据地图——全球填色地图
date: 2022-05-04T02:31:16.255Z
summary: |-
  地理空间可视化教程

  <https://cloud.tencent.com/developer/article/1091301>
draft: false
featured: false
tags:
  - 可视化
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
来源：https://cloud.tencent.com/developer/article/1091301

整个过程以及代码并没有太大差别，只要拿到世界地图素材，根据之前的代码，自己修改参数和指标名称以及引用路径，完全可以做出来（尽管并不一定理解每句代码的含义）。

**R语言环境:**

R x64 3.31/Rstudio 0.99.903/ggplot2 2.1.0

**代码过程：**

**加载功能所需支持的工具包：**

library(ggplot2)

library(plyr)

library("maptools")

**导入并整理世界地图地理信息数据：**

world_map <-readShapePoly("c:/rstudy/wold_map/World_region.shp")

x <- world_map@data #读取行政信息

xs <- data.frame(x,id=seq(0:250)-1) #含岛屿共251个形状

world_map1 <- fortify(world_map) #转化为数据框

world_map_data <- join(world_map1, xs, type = "full") #合并两个数据框

**导入指标文件数据并合并成作[图数据](https://cloud.tencent.com/product/konisgraph?from=10680)：**

mydata <- read.csv("C:/rstudy/wold_map/Region_map.csv") #读取指标数据，csv格式

world_data <- join(world_map_data, mydata, type="full") #合并两个数据框

**地图填充过程代码：**

这里还是通过调整映射方式参数：coord_map("ortho", orientation = c(30, 110, 0))可以变换地图的呈现视角：

**常见平面视角的全球地图填充：**

ggplot(world_data, aes(x = long, y = lat, group = group,fill = zhibiao1)) +

geom_polygon(colour="grey40") +

scale_fill_gradient(low="white",high="steelblue") + #指定渐变填充色，可使用RGB

theme( #清除不需要的元素

panel.grid = element_blank(),

panel.background = element_blank(),

axis.text = element_blank(),

axis.ticks = element_blank(),

axis.title = element_blank(),

legend.position = c(0.2,0.3)

)#平面地图

![](https://ask.qcloudimg.com/http-save/yehe-1598429/sxcu1trqmf.jpeg?imageView2/2/w/1620)

**立体空间地图：**（添加有映射方式参数coord_map）

ggplot(world_data, aes(x = long, y = lat, group = group,fill = zhibiao1)) +

geom_polygon(colour="grey40") +

coord_map("ortho", orientation = c(30, 110, 0))+

scale_fill_gradient(low="white",high="steelblue") + #指定渐变填充色，可使用RGB

theme( #清除不需要的元素

panel.grid = element_blank(),

panel.background = element_blank(),

axis.text = element_blank(),

axis.ticks = element_blank(),

axis.title = element_blank(),

legend.position = c(0.2,0.3)

)#映射成空间地图

![](https://ask.qcloudimg.com/http-save/yehe-1598429/x33bqrjfys.jpeg?imageView2/2/w/1620)

**以上的语法有几点需要提示一下：**

第一、代码中带#号后的文本是R语言认可的注释语句，带运行代码的时候不必清除，可以直接跑。

第二、由于全球地图呈现的信息比较丰富，所有的海岛和群岛信息全部都会上色，特别是北欧、北美（加拿大）、大洋洲这些多岛屿、群岛低于会有大量的密集分布的小岛，而填色代码在填充时，多边形线条填充为灰色，造成很多地区边界以及岛链出现大面积黑灰色。

看起来很不美观，所以如果可以将线条色设置为白色，这样效果会好些：geom_polygon(colour="white")

![](https://ask.qcloudimg.com/http-save/yehe-1598429/gm2h05f7mv.png?imageView2/2/w/1620)

但是这样做也会有不足，因为渐变色的色值范围是从(low="white",high="steelblue")连续过渡的，这样数值接近于零的地区会被填充为纯白，这样与边线的白色会混杂，导致局部地区边界难辨。

当然你也可以尝试用双色过渡。

![](https://ask.qcloudimg.com/http-save/yehe-1598429/f48xmymhsh.png?imageView2/2/w/1620)

我把渐变范围的低值与高值起点色和重点色替换成了：(low="DeepSkyBlue",high="OrangeRed")。

看起来比刚才由low="white"到high="steelblue"看着舒服一些。

但是通常来讲根据数据地图的填色规范：

指标都是正值，应该使用单色系连续渐变填充，只有在存在正负值类型的数据时，双色渐变才比较有意义。

所以用色规范还要遵循的，不过自己练着玩就没那么多将就了，可以想怎么弄就怎么弄。

<!--EndFragment-->