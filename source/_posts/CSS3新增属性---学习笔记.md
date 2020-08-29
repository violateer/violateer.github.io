---
title: CSS3新增属性
index_img: /img/css_mini.png
excerpt: 记录了我学习CSS3的学习笔记
hide: true
---

# CSS3新增

## 1.边框

##### border-radius	圆角边框

+ javascript语法：`object.style.borderRadius="top right bottom left"`
+ css语法：`border-radius: top right bottom left;`
  + 注：如果省略左下角，右上角是相同的。如果省略右下角，左上角是相同的。如果省略右上角，左上角是相同的。
  +  **border-top-left-radius**：左上角
  + **border-top-right-radius**：右上角
  + **border-bottom-right-radius**：右下角
  + **border-bottom-left-radius**：左下角

##### box-shadow  盒阴影

+ javascript语法：`object.style.boxShadow="h-shadow v-shadow blur spread color inset"`
+ css语法：`box-shadow: h-shadow v-shadow (blur spread color inset);`
  + **h-shadow**: 水平阴影的位置，允许负值
  + **v-shadow**: 垂直阴影的位置，允许负值
  + **blur**: 可选。模糊距离
  + **spread**: 可选。阴影的大小
  + **color**: 可选。阴影的颜色。在[CSS颜色值](https://www.runoob.com/cssref/css_colors_legal.aspx)寻找颜色值的完整列表
  + **inset**: 可选。从外层的阴影（开始时）改变阴影内侧阴影

##### border-image 边界图片  

+ IE浏览器不支持

+ [CSDN](https://blog.csdn.net/yzbben/article/details/52162563?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522159808344619724835843777%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=159808344619724835843777&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-52162563.first_rank_v2_rank_v25&utm_term=border-image&spm=1018.2118.3001.4187(https://blog.csdn.net/yzbben/article/details/52162563?ops_request_misc=%7B%22request%5Fid%22%3A%22159808344619724835843777%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=159808344619724835843777&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-52162563.first_rank_v2_rank_v25&utm_term=border-image&spm=1018.2118.3001.4187))

+ javascript语法：`object.style.borderImage="source slice width outset repeat|initial|inherit"`

+ css语法：`border-image: source slice width outset repeat|initial|inherit;`

  + **scource**: 图片地址---url(xxx)

  + **slice**: 图片切片

  + **width**: 边框图片宽度

  + **outset**: 边框图片外凸

  + **repeat**: 边框图片是否应重复（repeat）、拉伸（stretch）或铺满（round）

## 2.背景

##### background-image

+ javascript语法：`object.style.backgroundImage="url(xxx)"`
+ css语法：`background-image:url('xxx')`
  + **url('xxx')**: 图片文件地址
  
  + 下列属性参考‘渐变’
  
  + [linear-gradient()](https://www.runoob.com/cssref/func-linear-gradient.html): 创建一个线性渐变的 "图像"
  
  + [radial-gradient()](https://www.runoob.com/cssref/func-radial-gradient.html): 用径向渐变创建 "图像"。
  
  + [repeating-linear-gradient()](https://www.runoob.com/cssref/func-repeating-linear-gradient.html): 创建重复的线性渐变 "图像"。
  
  + [repeating-radial-gradient()](https://www.runoob.com/cssref/func-repeating-radial-gradient.html) :创建重复的径向渐变 "图像"
  
  + **inherit**: 指定背景图像应该从父元素继承

##### background-size

+ javascript语法：`object.style.backgroundSize="width height"`
+ css语法：`background-size: length|percentage|cover|contain;`
  + **length**: 设置背景图片高度和宽度。第一个值设置宽度，第二个值设置的高度。如果只给出一个值，第二个是设置为 **auto**(自动)
  + **precentage**: 将计算相对于背景定位区域的百分比。第一个值设置宽度，第二个值设置的高度。如果只给出一个值，第二个是设置为"auto(自动)"
  + **cover**:  此时会保持图像的纵横比并将图像==放大==成将完全覆盖背景定位区域的==最小==大小。
  + **contain**: 此时会保持图像的纵横比并将图像==缩小==成将适合背景定位区域的==最大==大小。

##### background-origin

+ [CSDN](blog.csdn.net/weixin_39256994/article/details/78698145?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522159808504119195188320865%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=159808504119195188320865&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v25-2-78698145.first_rank_v2_rank_v25&utm_term=background-origin&spm=1018.2118.3001.4187)
+ 当`background-attachment`属性设置为`fixed`时，`background-origin`属性会失效。

+ javascript语法： `object.style.backgroundOrigin="padding-box|border-box|content-box"`
+ css语法： `background-origin: padding-box|border-box|content-box;`
  + **padding-box**: 背景图像填充框的相对位置(padding区域)
  
  + **border-box**: 背景图像边界框的相对位置(border区域)
  
  + **content-box**: 背景图像的相对位置的内容框(content区域)
  
    ![background-origin](/img/background-origin.png)

##### background-clip

+ javascript语法： `object.style.backgroundClip="content-box"`

+ css语法：`background-clip: border-box|padding-box|content-box;`

  + **border-box**: 默认值,背景绘制在边框方框内(剪切成边框方框)

  + **padding-box**: 背景绘制在衬距方框内(剪切成衬距方框)

  + **content-box**: 背景绘制在内容方框内(剪切成内容方框)

    ![background-clip](/img/background-clip.png)

## 3.渐变

#####  linear-gradient 线性渐变

+ [linear-gradient()](https://www.runoob.com/cssref/func-linear-gradient.html):创建线性渐变
+ [repeating-linear-gradient()](https://www.runoob.com/cssref/func-repeating-linear-gradient.html): 创建重复的线性渐变

+ 向下/向上/向左/向右/对角方向
+ css语法：`background-image: linear-gradient(direction|angle, color-stop1, color-stop2, ...);`
  + **direction**: to right, to bottom left ......
  + **angle**: ![角度](/img/%E6%B8%90%E5%8F%98-%E8%A7%92%E5%BA%A6.jpg)

#####  radial-gradient 径向渐变

+ [radial-gradient()](https://www.runoob.com/cssref/func-radial-gradient.html):创建径向渐变

+ [repeating-radial-gradient()](https://www.runoob.com/cssref/func-repeating-radial-gradient.html) :创建重复的径向渐变 "图像"

+ 由它们的中心定义

+ css语法： `background-image: radial-gradient(shape size at position, start-color, ..., last-color);`

+ 颜色节点不均匀分布的径向渐变：`background-image: radial-gradient(red 5%, yellow 15%, green 60%);`

+ 设置形状---shape参数
  + **circle**---圆形, **ellipse**---椭圆形---默认值
  + 形状为圆形的径向渐变：`background-image: radial-gradient(circle, red, yellow, green);`
  
+ 设置渐变大小---size参数

  + **closest-side**

  + **farthest-side**

  + **closest-corner**

  + **farthest-corner**

  + 例：`background-image: radial-gradient(closest-side at 60% 55%, red, yellow, black);`

## 4.文本效果

##### text-shadow 文本阴影

+ javascript语法：`object.style.textShadow="h-shadow v-shadow blur color"`
+ css语法：`text-shadow: h-shadow v-shadow blur color;`
  + **h-shadow:**必需，水平阴影的位置，允许负值
  + **v-shadow**:必需，垂直阴影的位置，允许负值
  + **blur:**可选，模糊的距离
  + **color:**可选，阴影的颜色

##### box-shadow  盒子阴影

##### text-overflow 

##### word-wrap

##### word-break