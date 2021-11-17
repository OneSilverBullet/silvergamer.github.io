---
layout: post
title:  "Dilation & Depth of Field"
date:   2021-11-17
excerpt: "The post effects that you cant live without once you have used."
tag:
- Post-Processing 
- Screen Space
- Graphics
comments: true
paper: true
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



