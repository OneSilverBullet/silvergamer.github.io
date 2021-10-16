---
layout: post
title:  "Code Drawing World: Create PBR Software Rasterizer"
date:   2021-10-07
excerpt: "Create PBR Software Rasterizer from One Line C++ Code."
project: true
tag: [Graphics Pipeline, oftware Rasterizer, PBR]
comments: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/SoftR.png
---
    
<center>God said, "Let there be light," and there was light.</center>


### 序言：光栅化对图形学的重要意义

对我而言，图形学是一门创建虚拟世界的学问。目前的3A级别游戏是图形学登峰造极水平的最佳示例。上古卷轴系列为我们展现了白金塔下绝美的风景，巫师在惨淡的夕阳下给我们展现了一个人类与怪物共存的世界……游戏制作人在虚拟世界中尽情发挥自己的奇思妙想，让全世界各个角落的人们可以体验到完全不同的人生。这一切都归功于基于GPU的图形学技术的突破与发展。在我看来，图形学技术的研究者无异于世界的开发者，他们殚精竭虑考虑如何能够更好的模拟、创建一个更好的世界，如同创世者一般。而这也是我选择游戏引擎开发作为我的职业的原因。

如今图形学领域的知识瞬息万变。得益于GPU硬件技术的不断突破，实时光线追踪、动态全局光照的进展一日千里。每天都有实时性更好、性能更佳的新算法出现。而正如古语所云，罗马并非一日建成。理解图形学的基础算法对学习最新算法非常重要，而其中最为重要的算法就是光栅化算法。

光栅化算法的作用是我们所设计的虚拟物体映射到二维平面上，这个算法是近20年来几乎所有实时图形学算法的基石。 如今，这种算法被编码于GPU硬件当中，OpenGL与DirectX都有相关的接口可以调用。虽然光栅化算法已然成为了新生代程序员的“神的恩赐”，但是其思想仍然深刻地影响着业界。2016年的Siggraph中，光栅化思想被应用于场景物体的遮挡剔除[^1]；保守光栅化思想是时间抗锯齿[^2]的重要组成部分……

[^1]: <https://dl.acm.org/doi/abs/10.5555/2977336.2977340>
[^2]: <http://alextardif.com/TAA.html>

光栅化算法是一些游戏公司中引擎程序的培训项目。掌握光栅化算法是开启图形学大门的钥匙，这是我撰写此文的原因。本文详细阐述了光栅化思想，并提供一种光栅化算法在CPU端的实现。希望能够帮到需要的新人们。下面是软渲染器的代码链接：

<iframe src="https://github.com/OneSilverBullet/SilverGL-Soft_Render_Pipline" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>    

如果这篇文章帮到你了，请给我的项目一个Star。谢谢观赏。

### 概述
我们分为五个部分对光栅化相关内容进行分析。首先，我们来看一看渲染的流程，了解为什么要使用这一套流程来进行渲染。其次，我们将会介绍重心坐标法，这是光栅化算法的核心内容。在第三节介绍扫描线算法以及三角形裁剪算法，我们将会了解到光栅化算法的优化方案。在第四节，我们将介绍基于物理的渲染，我们会了解到每一个像素是如何着色的。最后我们将展示软光栅算法的效果与总结。在每一节中都提供了详尽的实现代码。

* 渲染管线原理
* 重心坐标法
* 扫描线算法及三角形裁剪
* 光照模型：基于物理的渲染
* 效果与总结


### 1.渲染管线原理

光栅化算法是基于一个反直觉的渲染流程运行的。认真的说，实时渲染管线的内容是比较违背人类直觉的，而光线追踪框架更加符合人类的直观感受。然而，迫于GPU性能的限制，过去二十年里，光线追踪渲染框架并未成为游戏引擎的通用解决方案。实时渲染管线脱颖而出，而光栅化算法成为了实时图形学应用的唯一解决方案。为了深入理解光栅化算法，了解实时渲染管线的流程至关重要。

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/RenderPipeline.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/RenderPipeline.png" align="center"></a>
    <figcaption>Render Pipeline in our Software Rasterizer.</figcaption>
</figure>


我们所设计的软光栅程序的渲染管线如上图所示。整个渲染流程由四个阶段构成：设计场景、顶点变换、三角形组装、光栅化。

试想一下，我们现在要渲染一个漂亮的场景，首先需要艺术家们通过3DMax、Maya等软件创建相应的场景模型。场景模型通常由大量顶点构成，而顶点由顶点属性描述。例如顶点的position，normal，uv以及构成每一个三角形所需要的三个顶点索引值。我们将渲染管线的整体视为一个函数，而这些顶点属性所构成的数据集合，就是这个函数的输入值。因此，设计场景是确定渲染管线输入的关键阶段，通常由团队中的艺术家完成这一阶段。

以.obj文件为例，

~~~ C++


~~~



### 2.重心坐标法

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/BCM.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/BCM.png" align="center"></a>
    <figcaption>Barycentric Coordinate Method.</figcaption>
</figure>

### 3.扫描线算法及三角形裁剪
<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/Scanline.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/Scanline.png" align="center"></a>
    <figcaption>Scanline Method.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/TriCutting.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/TriCutting.png" align="center"></a>
    <figcaption>Triangular Cutting.</figcaption>
</figure>


### 4.光照模型：基于物理的渲染

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/PBR.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/SoftRFig/PBR.png" align="center"></a>
    <figcaption>PBR Material.</figcaption>
</figure>


### 5.效果与总结
### 额外阅读