---
layout: post
title:  "Post Effects"
date:   2021-11-17
excerpt: "The post effects that you cant live without once you have used."
tag: [Graphics Pipeline]
comments: true
project: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/DOF.jpg
---
## Overview

Dilation and Depth of Field are both classic post-process algorithms. Their relation is close. So we will talk about them together.

## Dilation

### Conception

* Dilation dialtes or enlarges the brighter areas of an image while at the same time, contracts or shrinks the darker areas of an image. 
* Effect: make a pillowy look.
* Usage: use dilation for a glow/bloom effect or DOF.

### Implementation

{% highlight cpp %}
  int   size         = int(parameters.x);
  float separation   =     parameters.y;
  float minThreshold = 0.1;
  float maxThreshold = 0.3;
{% endhighlight %}

* size/separation: how dilated the image become. (larger size will increase the cost of performance.)
* minThreshold/maxThreshold: which parts of the image become dilated.

{% highlight cpp %}
vec2 texSize   = textureSize(colorTexture, 0).xy;
vec2 fragCoord = gl_FragCoord.xy;
fragColor = texture(colorTexture, fragCoord / texSize);
{% endhighlight %}
Sample the color in current fragment. (This is a classic code section)

{% highlight cpp %}
  float  mx = 0.0;
  vec4  cmx = fragColor;

  for (int i = -size; i <= size; ++i) {
    for (int j = -size; j <= size; ++j) {
      // For a rectangular shape.
      //if (false);

      // For a diamond shape;
      //if (!(abs(i) <= size - abs(j))) { continue; }

      // For a circular shape.
      if (!(distance(vec2(i, j), vec2(0, 0)) <= size)) { continue; }

      vec4 c =
        texture
          ( colorTexture
          ,   ( gl_FragCoord.xy
              + (vec2(i, j) * separation)
              )
            / texSize
          );

      float mxt = dot(c.rgb, vec3(0.3, 0.59, 0.11));

      if (mxt > mx) {
         mx = mxt;
        cmx = c;
      }
    }
  }
{% endhighlight %}

There is some points need to be interpretated:
* The window shape determine the shape of the dilated parts of the image.
* c is sample color in the neighbor fragments.
* mxt is the gray value. We can convert sample color to gray color by dotting the gray vector.
* cmt is the sample color which has the biggest grey value in this window.
* mx: is the max grey value in this window.



{% highlight cpp %}
 fragColor.rgb =
    mix
      ( fragColor.rgb
      , cmx.rgb
      , smoothstep(minThreshold, maxThreshold, mx)
      );
{% endhighlight %}

If the fragment color found is less than minThreshold, the fragment color will be unchanged. If the fragment color found is greater than maxThreshold, the fragment color is replaced with the brightest color found.


## Depth of Field

The Depth of Field is an effect you cant live without once you've used.

### Conceptions

* In Focus: Render the scene to a framebuffer in a normal way.
* Out of Focus: Render the scene in a blur way. For example, box blur, dilation.
* Bokeh: Dilate the out of focus texture, and use it as the out of focus input.

### Implementation


{% highlight cpp %}
  float minDistance = 1.0;
  float maxDistance = 3.0;
{% endhighlight %}

All positions below the minDistance will be completely in focus. All positions beyond the maxDistance will be completely out of focus.

{% highlight cpp %}
  vec4 focusColor      = texture(focusTexture, texCoord);
  vec4 outOfFocusColor = texture(outOfFocusTexture, texCoord);
{% endhighlight %}

The in focus color and out of focus color.

{% highlight cpp %}
  vec4 position = texture(positionTexture, texCoord);
{% endhighlight %}


The vertex position in view space.

{% highlight cpp %}
  float blur =
    smoothstep
      ( minDistance
      , maxDistance
      , abs(position.y - focusPoint.y)
      );
      
  fragColor = mix(focusColor, outOfFocusColor, blur);
{% endhighlight %}

We can determine current fragment color by mixing the focus color and out of focus color, according to the blur factor. The blur factor can be calculated by the depth difference between the focus point and current fragment.




## Motion Blur

### Overview

The motion blur technique is divide into two categories. The less involved implementation will only blur the screen in relation with camera. The more involved implementation will blur any moving objcet even with camera remaining still.

This paper talks about the less involved version, the principle is the same with the more involved version.

### Implementation

#### Preparation

{% highlight cpp %}
uniform sampler2D positionTexture;
uniform sampler2D colorTexture;
uniform mat4 previousViewWorldMat;
uniform mat4 worldViewMat;
uniform mat4 lensProjection;
{% endhighlight %}

* positionTexture: vertex position in view space.
* colorTexture: normal render texture.
* previousViewWorldMat: the previous frame's view to world matrix.
* worldViewMat: the current world to view matrix.
* lensProjection: the camera lens' projection matrix.

{% highlight cpp %}
uniform int size;
uniform float separation;
{% endhighlight %}

The adjustment factors.

* size: how many samples are taken along the blur direction.
* separation: increases the amount of blur at the cost of accuracy.

#### Core

{% highlight cpp %}
  vec2 texSize  = textureSize(colorTexture, 0).xy;
  vec2 texCoord = gl_FragCoord.xy / texSize;

  vec4 position1 = texture(positionTexture, texCoord);
  vec4 position0 = worldViewMat * previousViewWorldMat * position1;
{% endhighlight %}

Firstly, we calculate the texCoord in current fragment. 

* position1: where the things are now, sample the current vertex position.
* position0: this fragment's previous interpolated vertex position.


{% highlight cpp %}
  position0      = lensProjection * position0;
  position0.xyz /= position0.w;
  position0.xy   = position0.xy * 0.5 + 0.5;

  position1      = lensProjection * position1;
  position1.xyz /= position1.w;
  position1.xy   = position1.xy * 0.5 + 0.5;=
{% endhighlight %}

Get the fragment's coordinate in screen space.

{% highlight cpp %}
  vec2 direction    = position1.xy - position0.xy;

  if (length(direction) <= 0.0) { return; }
{% endhighlight %}

Get the direction from previous frame's uv to current frame's uv.

{% highlight cpp %}
 direction.xy *= separation;

  vec2 forward  = texCoord;
  vec2 backward = texCoord;

  float count = 1.0;

  for (int i = 0; i < size; ++i) {
    forward  += direction;
    backward -= direction;

    fragColor +=
      texture
        ( colorTexture
        , forward
        );
    fragColor +=
      texture
        ( colorTexture
        , backward
        );

    count += 2.0;
  }

  fragColor /= count;
{% endhighlight %}
 From current fragment, we will extend the forward direction and backward direction.
 Finally, we will make an average among these sample colors.
 
 
## Pixelization

### Overview
 
 Pixelization helps you to save you time by not having to create all of the pixel art.
 
 ### Implementation
 
{% highlight cpp %}
 int pixelSize = 5;
{% endhighlight %}
 * pixelSize: the level of the scene's pixelization.
 
{% highlight cpp %}
  float x = int(gl_FragCoord.x) % pixelSize;
  float y = int(gl_FragCoord.y) % pixelSize;

  x = floor(pixelSize / 2.0) - x;
  y = floor(pixelSize / 2.0) - y;

  x = gl_FragCoord.x + x;
  y = gl_FragCoord.y + y;
  
  fragColor = texture(colorTexture, vec2(x, y) / texSize);
{% endhighlight %}

In one word, we will sample the screen color in a block way. The center of the window fragments determine the color the other fragments in their window.


