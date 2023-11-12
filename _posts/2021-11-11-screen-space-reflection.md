---
layout: post
title:  "Screen Space Reflection"
date:   2021-11-11
excerpt: "Fake Global Illumination: Achieve effect comparable to global illumination with minimal cost."
tag:
- Render 
- Screen Space
- Graphics
- Global Illumination
comments: true
graphics: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/DOF.jpg
---

## Overview

* SSR works by reflectingb the screen image onto itself using only iteself.
* SSR reflect a ray from some point on your screen to some other point on your screen.


### Ray Marching

* Ray Marching is the process of iteratively extending or contracting the length or magnitude of some vector in order to probe or sample some space for information.

### Screen Space Reflection
* SSR is the process of tracing the light ray's path in reverse. It tries to find the reflected point the light ray bounced off of and hit the current fragment.


### Algorithm Thinking

* With each iteration, the algorithm samples the scene's positions or depths, along the reflection ray, asking each time if the ray intersected with the scene's geometry. 
* If there is an intersection, that position is a potential condidate for being reflected by the current fragment.

## Implementation

### Vertex Normal

* Target: Reflection follows the ripples in the water versus the more mirror like reflection shown earlier.
* Solution: Normal Mapped.

### Position Transformation

* SSR goes back and forth between the **screen space** and **view space**.
* From view space to clip space: projection matrix.
* From clip space to uv space.
* In UV space, you can sample a vertex/fragment position from the scene which will be the closest position in the scene to your sample.

### Reflected UV Coordinate

* Target: Computing a reflected UV coordinate for each screen fragment.
* These calculated uv coordinates can be saved to a framebuffer, and used later when the scene has been rendered.


{% highlight cpp %}
uniform mat4 cameraProjection;
uniform sampler2D viewSpacePositionTex;
uniform sampler2D normalTex;
{% endhighlight %}

The Data needed for SSR.

{% highlight cpp %}
float maxDistance = 15;
float resolution = 0.3;
int steps = 10;
float thickness = 0.5;
{% endhighlight %}

Some Defination:

**First Pass**: The first pass is to find a point along the ray's direction where the ray enters or goes behind some geometry in the scene.

**Second Pass**: The second Pass is to find the exact point along the reflection ray's direction where the ray immediately hits or intersects with some geometry in the scene.

* maxDistance: How far a fragment can reflect. In other words, it controls the length of the reflection ray.
* resolution: How many fragments are skipped while traveling or marching the reflection ray during the first pass.
* steps: how many iterations occur during the second pass.
* thickness: It controls the cutoff between what counts as a possible reflection and what does not.

{% highlight cpp %}
vec2 texSize = textureSize(viewSpacePositionTex, 0).xy;
vec2 texCoord = gl_FragCoord.xy / texSize;
vec4 positionFrom = texture(viewSpacePositionTex, texCoord);
vec3 unitPositionFrom = normalize(positionFrom.xyz);
vec3 normal = normalize(texture(normalTexture, texCoord).xyz);
vec3 pivot = normalize(reflect(unitPositionFrom, normal));
{% endhighlight %}

* positionFrom: the vector from the camera to the current fragment.
* normal: the vector pointing in the direction of the interpolated vertex normal for the current fragment.
* pivot: the reflection ray or vector pointing in the reflected direction of the positionFrom vector.

{% highlight cpp %}
vec4 startViewPoint = vec4(positionFrom.xyz + (pivot * 0), 1);
vec4 endViewPoint = vec4(positionFrom.xyz + (pivot * maxDistance), 1);
{% endhighlight %}

Get the start and end point of reflection ray in view space.

{% highlight cpp %}
vec4 startFrag = startViewPosition;
startFrag = cameraProjection * startFrag;
startFrag.xyz /= startFrag.w;
startFrag.xy = startFrag.xy * 0.5 + 0.5;
startFrag.xy *= texSize;

vec4 endFrag = endViewPosition;
endFrag = cameraProjection * endFrag;
endFrag.xyz /= endFrag.w;
endFrag.xy = endFrag.xy * 0.5 + 0.5;
endFrag.xy *= texSize;
{% endhighlight %}

Project and transform the start view position and end view position to the screen space. We will travel along this line using it to sample the fragment positions stored in the position fragment textures.

Screen Space Reflection uses marching in screen space, which is more efficiently than marching in view space.

{% highlight cpp %}
vec2 frag = startFrag.xy;
uv.xy = frag / texSize;
{% endhighlight %}

Converting the fragment position to UV coordinate.

{% highlight cpp %}
float deltaX = endFrag.x - startFrag.x;
float deltaY = endFrag.y - startFrag.y;
{% endhighlight %}

Calculate the delta or difference between the X and Y coordinates of the end and start fragments.

{% highlight cpp %}
float useX = abs(deltaX) >= abs(deltaY) ? 1 : 0;
float delta = mix(abs(deltaY), abs(deltaX), useX) * clamp(resolution, 0, 1);
{% endhighlight %}
To handle all of various different ways, we should keep track of the larger difference. The larger difference help us to determine how much to travel in X and Y direction each iteration.

{% highlight cpp %}
vec2 increment = vec2(deltaX, deltaY) / max(delta, 0.001);
{% endhighlight %}

The increment is the step we move every iteration.

{% highlight cpp %}
float search0 = 0;
float search1 = 0;
current_position_x = start_x * (1 - search1) + end_x * search1;
current_position_y = start_y * (1 - search1) + end_y * search1;
{% endhighlight %}

* search1 is  the linear interpolation factor.
* search0 is used to remember the last position on the line where the ray missed or didnt intersect with any geometry.

{% highlight cpp %}
int hit0 = 0;
int hit1 = 0;
{% endhighlight %}

* hit0: there was an intersection during the first pass.
* hit1: there was an intersection during the second pass.

{% highlight cpp %}
float viewDistance = startView.y;
float depth = thickness;
{% endhighlight %}

* viewDistance: how far away from the camera to the current fragment.
* depth: the view distance difference between the current ray point and scene position. It tells us how far behind or in front of the scene the ray currently is.

{% highlight cpp %}
//The first pass: to get a cursory point.
for (int i = 0; i < int(delta); ++i) {
    frag += increment;
    uv.xy = frag / texSize; 
    positionTo = texture(positionTexture, uv.xy);

    search1 =
      mix
        ( (frag.y - startFrag.y) / deltaY
        , (frag.x - startFrag.x) / deltaX
        , useX
        );

    search1 = clamp(search1, 0.0, 1.0);
    viewDistance = (startView.y * endView.y) / mix(endView.y, startView.y, search1);
    depth        = viewDistance - positionTo.y;

    if (depth > 0 && depth < thickness) {
      hit0 = 1;
      break;
    } else {
      search0 = search1;
    }
}
{% endhighlight %}

The first pass is the process of advancing the current fragment position closer to the end fragment. We will find a cursory hit point for the ray. The first pass contains four steps:

- Calculate the distance from the camera to current fragment. Sample from the viewPositionTexture: positionTo.y.
- Search1 is the percentage or portion of the line the current position represents.
- Using Search1, viewDistance can be interpolated for the current position. 
- Depth is the distance between the fragment and the point on ray line.
- We can check whether the point hits geometry by Depth.
- The condition: depth > 0 && depth < thickness means that, the distance between ray point and camera is little longer than the distance between fragment and camera.
- Be attention, the viewdistance is calculated by  perspective-correct interpolation.
- Seach0 record the last iteration

{% highlight cpp %}
//The second pass
search1 = search0 + ((search1 - search0) / 2.0);
steps *= hit0;
for (i = 0; i < steps; ++i) {
    frag = mix(startFrag.xy, endFrag.xy, search1);
    uv.xy = frag / texSize;
    positionTo = texture(positionTexture, uv.xy);

    viewDistance = (startView.y * endView.y) / mix(endView.y, startView.y, search1);
    depth = viewDistance - positionTo.y;

    if (depth > 0 && depth < thickness) {
      hit1 = 1;
      search1 = search0 + ((search1 - search0) / 2);
    } else {
      float temp = search1;
      search1 = search1 + ((search1 - search0) / 2);
      search0 = temp;
    }
}
{% endhighlight %}

- The second pass is a refine process to find the exactly point that the ray hit the geometry.
- Search1 is initialized as a value between the search0 and search1.
- The whole process is just like a dicotomizing search. In steps iterations, we can get a refinement for the hit point position.

{% highlight cpp %}
//final step: calculate the visible of the current hit point.
  float visibility =
      hit1 //step1
    * positionTo.w //step2
    * ( 1
      - max
         ( dot(-unitPositionFrom, pivot)
         , 0
         )
      ) //step3
    * ( 1
      - clamp
          ( depth / thickness
          , 0
          , 1
          )
      )//step4
    * ( 1
      - clamp
          (   length(positionTo - positionFrom)
            / maxDistance
          , 0
          , 1
          )
      )//step5
    * (uv.x < 0 || uv.x > 1 ? 0 : 1)
    * (uv.y < 0 || uv.y > 1 ? 0 : 1);//step6

  visibility = clamp(visibility, 0, 1);
{% endhighlight %}


The visible of the reflection point is affected by five factors:
* The visible is ranges from zero to one.
* step1: hit1 decide whether the ray hit geometry.
* step2: if w is zero, there is no scene position.
* step3: The reflection ray points torward the camera, and hit something. This situation is that, the ray hit some geometry's backward.
* step4: We can find exact point where the reflection ray first intersects with the scene's geometry. So we fade out the reflection the further it is from the intersection point.
* step5: Fade out the reflection based on how far way the reflected point is from the initial start point.
* step6: the point out of clip space.

{% highlight cpp %}
  uv.ba = vec2(visibility);
  fragColor = uv;
{% endhighlight %}


The final output color is like above.

### Specular Map

Specular Map show us which fragments need screen space reflection.
Specular Map plays a role of mask texture in SSR. Just like gbuffer, we can get a specular map by deffered shadering.

In my opinion, to make ssr support PBR material, we should calculate specular texture in PBR pipeline firstly. The specular texture can be used both in SSR merge stage and deffered lighting stage.

### Reflected Scene Colors

Once we get the reflected uv texture, looking up the reflected colors is fairly easy. 

{% highlight cpp %}
//get current fragment's uv
vec2 texSize  = textureSize(uvTexture, 0).xy;
vec2 texCoord = gl_FragCoord.xy / texSize;

//with reflected uv, we can get reflected color.
vec4 uv    = texture(uvTexture,    texCoord);
vec4 color = texture(colorTexture, uv.xy);
  
//sample the ssr visible
float alpha = clamp(uv.b, 0, 1);

//get final color
fragColor = vec4(mix(vec3(0), color.rgb, alpha), alpha); 
{% endhighlight %}

### Blurred Reflected Scene Colors

Blur the reflected colors and store the blur texture in a framebuffer. The blurred reflected colors are used for surfaces that have a less than mirror.


### Final SSR Color

The final SSR effect is influenced by reflected color and blur reflected color.

{% highlight cpp %}
//Input texture.
uniform sampler2D colorTexture; 
uniform sampler2D colorBlurTexture;
//Mask texture.
uniform sampler2D specularTexture;

//Smaple
vec4 specular  = texture(specularTexture,  texCoord);
vec4 color     = texture(colorTexture,     texCoord);
vec4 colorBlur = texture(colorBlurTexture, texCoord);

//get specular amount
float specularAmount = dot(specular.rgb, vec3(1)) / 3;

//if specular is error. dot(specular.rgb, vec3(1)) == (specular.r + specular.g + specular.b);
if (specularAmount <= 0) { fragColor = vec4(0); return; }

//get the roughness. In PBR, the step can be sample in roughness texture.
float roughness = 1 - min(specular.a, 1);

//final color 
  fragColor = mix(color, colorBlur, roughness) * specularAmount;
{% endhighlight %}

Make a summary, the ssr is affected by these factors:
- roughness: control the final color between the color and colorblur.
- specular: calculated by specular vector.
