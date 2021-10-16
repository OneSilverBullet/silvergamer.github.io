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

光栅化算法的作用是我们所设计的虚拟物体映射到二维平面上，这个算法是近20年来几乎所有实时图形学算法的基石。 如今，这种算法被编码于GPU硬件当中，OpenGL与DirectX都有相关的接口可以调用。虽然光栅化算法已然成为了新生代程序员的“神的恩赐”，但是其思想仍然深刻地影响着业界。2016年的Siggraph中，光栅化思想被应用于场景物体的遮挡剔除[1]；保守光栅化思想是时间抗锯齿[2]的重要组成部分……

光栅化算法是一些游戏公司中引擎程序的培训项目。掌握光栅化算法是开启图形学大门的钥匙，这是我撰写此文的原因。本文详细阐述了光栅化思想，并提供一种光栅化算法在CPU端的实现。文章分为四个部分进行讲解。本章讲述了光栅化算法在图形学中的重要性；第一节讲述了与光栅化算法相关的理论知识；第二节讲述了基于CPU端的算法实现方案，并给出了详尽的代码实现过程；第三节给出了光栅化算法的实现效果以及总结。希望能够帮到需要的新人们。

如果这篇文章帮到你了，请给我的项目一个Star。谢谢观赏。


<figure class="half">
    <a href="/_img/SoftRFig/RenderPipeline.png"><img src="/_img/SoftRFig/RenderPipeline.png"></a>
    <figcaption>Render Pipeline in our Software Rasterizer.</figcaption>
</figure>

### 光栅化原理

在这一节当中，



#### 渲染管线原理
#### 重心坐标法
#### 扫描线算法及三角形裁剪
#### 光照模型：基于物理的渲染
### 实现

#### 渲染管线设计

~~~ C++
//绘制单个模型
void SoftwareRender::drawTriangularMesh(Model * currentModel)
{
	Mesh* triMesh = currentModel->getMesh();
	std::vector<VECTOR3I>* pos_indices = &triMesh->fVertexIndices;
	std::vector<VECTOR3I>* uv_indices = &triMesh->fTextureIndices;
	std::vector<VECTOR3I>* nor_indices = &triMesh->fNormalsIndices;
	std::vector<VECTOR3D>* fNormals = &triMesh->fNormals;

	std::vector<VECTOR3D>* pos = &triMesh->vertices;
	std::vector<VECTOR3D>* uvs = &triMesh->texels;
	std::vector<VECTOR3D>* normals = &triMesh->normals;
	std::vector<VECTOR3D>* tangents = &triMesh->tangents;
	int numFaces = triMesh->numFaces;


	MATRIX4X4 worldToObject = (*(currentModel->getModelMatrix())).GetInverseTranspose();

	//初始化Shader信息
	software_camera->BuildViewMatrix();
	viewMatrix = software_camera->camera_viewMatrix;
	modelMatrix = *currentModel->getModelMatrix();
	projectMatrix = software_camera->camera_projectMatrix;
	curTexture = currentModel->getAlbedo()->sampler;
	
	PBRShader pbrShader;
	//1. 初始化图像
	pbrShader.albedo = currentModel->getAlbedo();
	pbrShader.normal = currentModel->getNormal();
	pbrShader.ambientO = currentModel->getAO();
	pbrShader.roughness = currentModel->getRoughness();
	pbrShader.metal = currentModel->getMetallic();
	//2. 初始化灯光
	VECTOR3D* lightPositions = new VECTOR3D[software_lightnum];
	VECTOR3D* lightColors = new VECTOR3D[software_lightnum];
	for (int i = 0; i < software_lightnum; ++i)
	{
		lightColors[i] = software_lights[i].color;
		lightPositions[i] = software_lights[i].position;
	}
	pbrShader.lightCol = lightColors;
	pbrShader.lightPos = lightPositions;
	pbrShader.cameraPos = software_camera->getPosition();
	pbrShader.numLights = software_lightnum;
	//3. 初始化必要矩阵
	software_camera->BuildViewMatrix();
	pbrShader.MV = (software_camera->camera_viewMatrix)*(*currentModel->getModelMatrix());
	pbrShader.MVP = software_camera->camera_projectMatrix * pbrShader.MV;
	pbrShader.V = software_camera->camera_viewMatrix;
	pbrShader.M = *currentModel->getModelMatrix();
	pbrShader.N = (pbrShader.M.GetInverseTranspose());
	pbrShader.N.Transpose();
	

	for (int j = 0; j < numFaces; ++j)
	{
		//初始化当前模型第j个面的相关信息：位置，法线，纹理，切线
		VECTOR3D trianglePrim[3], normalPrim[3], uvPrim[3], tangentPrim[3];
		//获取相关模型第j个面的相关index。
		VECTOR3I f = (*pos_indices)[j];
		VECTOR3I n = (*nor_indices)[j];
		VECTOR3I u = (*uv_indices)[j];

		VECTOR4D* lightDirs = new VECTOR4D[software_lightnum * 3];
		pbrShader.lightDirVal = lightDirs;
		//获取当前三角形的完整几何信息，三个顶点的信息
		packDataIntoTris(f, *pos, trianglePrim);
		packDataIntoTris(n, *normals, normalPrim);
		packDataIntoTris(u, *uvs, uvPrim);
		packDataIntoTris(f, *tangents, tangentPrim);
		VECTOR4D firtsPoint = VECTOR4D(trianglePrim[0]);
		//检查当前面是否是正对着视角的，否则就筛除。
		VECTOR4D camPos = software_camera->getPosition();
		if (SilverGL::cullFace((*fNormals)[j], firtsPoint, worldToObject, camPos, CULL_BACK))
		{
			continue;
		}
		//构建三角形
		Vertex modelA(trianglePrim[0], normalPrim[0], uvPrim[0], tangentPrim[0]);
		Vertex modelB(trianglePrim[1], normalPrim[1], uvPrim[1], tangentPrim[1]);
		Vertex modelC(trianglePrim[2], normalPrim[2], uvPrim[2], tangentPrim[2]);
		Triangle triangle(modelA, modelB, modelC);
		//绘制当前三角形
		SilverGL::drawFace(renderTarget->front, renderTarget->depth, pbrShader, &triangle);

		delete[] lightDirs;
	}

	delete[] lightPositions;
	delete[] lightColors;
}
~~~

#### 软光栅化算法
#### 在CPU上设计Shader系统
### 效果与总结
### 额外阅读



<iframe src="https://github.com/OneSilverBullet/SilverGL-Soft_Render_Pipline" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>    
      
## Overview
* Fork the [Moon repo](https://github.com/TaylanTatli/Moon/fork)
* Edit `_config.yml` file.
* Remove sample posts from `_posts` folder and add yours.
* Edit `index.md` file in `about` folder.
* Change repo name to `YourUserName.github.io`    
     
That's all.

## Preview

{% capture images %}
	https://cloud.githubusercontent.com/assets/754514/14509716/61ac6c8e-01d6-11e6-879f-8308883de790.png
	https://cloud.githubusercontent.com/assets/754514/14509717/61ad05ae-01d6-11e6-85ae-5a817dd8763b.png
	https://cloud.githubusercontent.com/assets/754514/14509714/61a89708-01d6-11e6-8fcd-74b002a060df.png
{% endcapture %}
{% include gallery images=images caption="Screenshots of Moon Theme" cols=3 %}

---

{% capture images %}
	https://cloud.githubusercontent.com/assets/754514/14509718/61b09a20-01d6-11e6-8da1-4202ae4d83cd.png
	https://cloud.githubusercontent.com/assets/754514/14509715/61aa9d00-01d6-11e6-81a6-c6837edf2e84.png
{% endcapture %}
{% include gallery images=images caption="Moon Theme on Small Screen Size" cols=2 %}      
      
See a [live version of Moon](http://taylantatli.github.io/Moon) hosted on GitHub.      

## Site Setup
A quick checklist of the files you’ll want to edit to get up and running.    

### Site Wide Configuration
`_config.yml` is your friend. Open it up and personalize it. Most variables are self explanatory but here's an explanation of each if needed:

#### title

The title of your site... shocker!

Example `title: My Awesome Site`

#### bio

The description to show on your homepage.

#### description

The description to use for meta tags and navigation menu.

#### url

Used to generate absolute urls in `sitemap.xml`, `feed.xml`, and for generating canonical URLs in `<head>`. When developing locally either comment this out or use something like `http://localhost:4000` so all assets load properly. *Don't include a trailing `/`*.

Examples:

{% highlight yaml %}
url: http://taylantatli.me/Moon
url: http://localhost:4000
url: //cooldude.github.io
url:
{% endhighlight %}

#### reading_time

Set true to show reading time for posts. And set `words_per_minute`, default is 200.

#### logo
Your site's logo. It will show on homepage and navigation menu. Also used for twitter meta tags.

#### background
Image for background. If you don't set it, color will be used as a background.

#### Google Analytics and Webmaster Tools

Google Analytics UA and Webmaster Tool verification tags can be entered in `_config.yml`. For more information on obtaining these meta tags check [Google Webmaster Tools](http://support.google.com/webmasters/bin/answer.py?hl=en&answer=35179) and [Bing Webmaster Tools](https://ssl.bing.com/webmaster/configure/verify/ownership) support.

#### MathJax
It's enabled. But if you don't want to use it. Set it false in  `_config.yml`.

#### Disqus Comments
Set your disqus shortname in `_config.yml` to use comments.

---

### Navigation Links

To set what links appear in the top navigation edit `_data/navigation.yml`. Use the following format to set the URL and title for as many links as you'd like. *External links will open in a new window.*

{% highlight yaml %}
- title: Home
  url: /

- title: Blog
  url: /blog/

- title: Projects
  url: /projects/

- title: About
  url: /about/

- title: Moon
  url: http://taylantatli.me/Moon
{% endhighlight %}

---

## Layouts and Content

Moon Theme use [Jekyll Compress](https://github.com/penibelst/jekyll-compress-html) to compress html output. But it can cause errors if you use "linenos" (line numbers). I suggest don't use line numbers for codes, because it won't look good with this theme, also i didn't give a proper style for them. If you insist to use line numbers, just remove `layout: compress` string from layouts. It will disable compressing.

### Feature Image

You can set feature image per post. Just add `feature: some link` to your post's front matter.

```
feature: /assets/img/some-image.png
or
feaure: http://example.com/some-image.png
```    
 This also will be used for twitter card:

![Moon Twitter Card](https://cloud.githubusercontent.com/assets/754514/14509719/61c5751c-01d6-11e6-8c29-ce8ccad149bf.png)

### Comments
To show disqus comments for your post add `comments: true` to your post's front matter.

---

## Questions?

Found a bug or aren't quite sure how something works? By all means [file a GitHub Issue](https://github.com/TaylanTatli/Moon/issues/new). And if you make something cool with this theme feel free to let me know.

---

## License

This theme is free and open source software, distributed under the MIT License. So feel free to use this Jekyll theme on your site without linking back to me or including a disclaimer.
