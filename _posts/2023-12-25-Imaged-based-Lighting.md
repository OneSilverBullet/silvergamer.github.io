---
layout: post
title:  "Image Based Lighting"
date:   2023-12-25
excerpt: "The basic conception of real-time global illumination."
tag:
- Computer Graphics 
graphics: true
feature: https://github.com/OneSilverBullet/SilverGamer.GitHub.io/blob/gh-pages/_img/graph/head.png



---

## 1. Image based Lighting Overview

### 1.1 Imaged based Lighting Conception

In image based lighting, we consider the surrounding environments as a whole light source. 
In general, we use the **cube map** to present the surrounding environment, each pixel of the cube map is a light source.
* In this way, we can capture the environment global illumination.
* The cube map encodes the surrounding environment lighting.

Let's consider some basic conceptions:
* irradiance(E): the light of environment lighting that contributes to fragment.
* radiance(L_out): the light scattered from the fragment.

Then we introduce the rendering equation, which calculate **the integral of upper hemisphere** of **the input light wi**.

$$L_o(p, w_o) = \int_0^Ω (k_d\frac{c}{π} + k_s\frac{DFG}{4(w_o·n)(w_i·n)})L_i(p, w_i)n·w_idw_i$$

In the view of IBL, we should consider each texel in the cube map as a light source. **If we sample the environment cube map in direction wi, we can get the radiance with the help of rendering equation.**


### 1.2  IBL Formula Decomposition

Question: As we can see, the rendering equation is really complex, which means that it is impossible to calculate radiance in real time directly. 

Solution: The rendering equation can be separated into two parts: **diffuse part** and **specular part**. In modern game engine techonologies, we can get the different part lighting by different methods. 
* Diffuse part: we can use global illumination algorithm, such as Dynamic Diffuse Global Illumination.
* Specular part: reflection probe, Screen Space Reflection, Screen Space Global Illumination.

The rendering equation is updated as follows:

$$L_o(p, w_o) = \int_0^Ω k_d\frac{c}{π} L_i(p, w_i)n·w_idw_i + \int_0^Ω  k_s\frac{DFG}{4(w_o·n)(w_i·n)}L_i(p, w_i)n·w_idw_i$$

The IBL five types in mordern game engine.

As for diffuse global illumination part:
* Distant Light Probles:
    * Used to obtain lighting information at infinite distance.
    * Includes the sky, distant terrain features, buildings, etc.
    * Obtained through the engine and stored in the form of HDRI. 
* Local Light Probes
    * capture the environment light information in local position
    * includes the surrounding geometries
    * more accurate than the distant light probes

As for specular global illumination part:
* Reflection Probe
    * encode the specular lighting information about surrounding environment 
* Planar Reflection
    * encode the street, floor reflection information
* Screen Space Reflection
    * get the screen reflection based on ray marching algorithm with depth buffer.
    * expensive but nice visual effect.

In general, the IBL methods can be considered as static and dynamic. The dynamic IBL is calculated the environment lighting in real-time.
* The dynamic IBL always used for simulation of day and night circles.
* Recalculate Distant Light Probes in a load balancing manner.
* Planar and SSR are both dynamic methods.

To calculate the different part of the rendering equation, we apply different mathematics tools:
* Diffuse Part: Irradiance Map and Spherical Harmonic.
* Reflection Part: Pre-filtered Importance Sampling, Split-Sum Approximation.

### 1.3 HDR and IBL

In pbr pipeline, we should consider the **HIGH DYNAMIC RANGE** environment lighting.

HDR maps allow you to specify color values from 0.0-1.0, which gives the light the correct color intensity. HDR is stored in an Equiretangular Map. Horizontal viewing angle resolution is higher, bottom and top resolution is lower, and **most meaningful lightings are near the horizontal viewing angle**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/hdr.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/hdr.png" align="center"></a>
    <figcaption>the hdr graph.</figcaption>
</figure>

## 2. IBL: Diffuse Part


### 2.1 Diffuse Part Conception


Question: As we can see, the lambert item in the diffuse part integral does not depende on integral variable. Hence, we can pay attention on the integral calculation about $L_i$. In another words, we should calculate the **irradiance map**.

$$L_o(p, w_o) = k_d\frac{c}{π}\int_0^Ω  L_i(p, w_i)n·w_idw_i$$


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/hemi.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/hemi.png" align="center"></a>
    <figcaption>the hemisphere integral.</figcaption>
</figure>


Solution: we should pre-calculate a new cube map, we store diffuse reflection integration results in each sampling direction, which are obtained by convolution
calculated. This cube map is the irradiance map, which record the **precalculated sum of all indirect diffuse reflection lights that can hit the wo surface**.

Assume we have calculated the irradiance map. In the real-time rendering shader, we can sample the diffuse irradiance in shader program as follows:

```
float3 irradiance = texture(irradianceMap, N);
```


### 2.2 Irradiance Map

The origin environment map is an HDR cube map converted from an equirectangular map. The rendering equation integral is about the solid angle w. To simplify the calculation process, **polar coordinates** are used instead of **solid angles**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/solid.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/solid.png" align="center"></a>
    <figcaption>the solid angle.</figcaption>
</figure>

A simple integration idea: for each texel of the cube map, a fixed number of sampling vectors are generated within the Ω range of the direction represented by the texel, and the sampling results are averaged.

$$L_o(p, φ_o, θ_o) = \frac{c}{π}\int_0^{2π} \int_0^{\frac{1}{2π}}  L_i(p, φ_i, θ_i)cos(θ)sin(θ)dφdθ$$

The implementation in GLSL:

```
vec3 irradiance = vec3(0.0);  

vec3 up    = vec3(0.0, 1.0, 0.0);
vec3 right = normalize(cross(up, normal));
up         = normalize(cross(normal, right));

float sampleDelta = 0.025;
float nrSamples = 0.0; 
for(float phi = 0.0; phi < 2.0 * PI; phi += sampleDelta)
{
    for(float theta = 0.0; theta < 0.5 * PI; theta += sampleDelta)
    {
        // spherical to cartesian (in tangent space)
        vec3 tangentSample = vec3(sin(theta) * cos(phi),  sin(theta) * sin(phi), cos(theta));
        // tangent space to world
        vec3 sampleVec = tangentSample.x * right + tangentSample.y * up + tangentSample.z * N; 

        irradiance += texture(environmentMap, sampleVec).rgb * cos(theta) * sin(theta);
        nrSamples++;
    }
}
irradiance = PI * irradiance * (1.0 / float(nrSamples));
```



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/diffuse.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/diffuse.png" align="center"></a>
    <figcaption>the irradiance map.</figcaption>
</figure>




This process is stored in the Cubemap in a pre-calculated manner. At the same time, the diffuse reflection exists as an independent item and is directly added to the final result, to obtain the environment diffuse effect.

```
vec3 kS = fresnelSchlickRoughness(max(dot(N,V),0.0),F0, roughness);
vec3 kD = 1.0 - kS;
vec3 irradiance = texture(irradianceMap, N).rgb;
vec3 diffuse = irradiance * albedo;
vec3 ambient = (kD * diffuse) * ao;
```

As for the reason about calculation about fresnel item, we should consider the influence of roughness to the environment diffuse lighting. A material with high roughness always have weak reflection effect.

```
vec3 fresnelSchlickRoughness(float cosTheta, vec3 F0, float roughness){
    return F0 + (max(vec3(1.0 - roughness), F0) - F0) * pow(1.0 - cosTheta, 5.0);
}
```

### 2.3 Light Probe

In the real-time applictaion, such as game, a single irradiance map is not enough for the complex environment diffuse lighting. To represent the complex environment diffuse lighting, we need to arrange multiple light probes around the scene. Each light probe contain a irradiance map.

Question: However, this method is waste a great number of memory. We should consider another method to decrease the cost of memory.

Solution: Considering sampling from a texture is expensive in computer graphics, irradiance can be perfectly approximated using SH Decomposition. Sampling through SH to obtain irradiance is better than Cubemap, which is several orders of magnitude faster. 

The Decomposition process of SH is approximated by Fourier transform, which represents the signal as an orthogonal basis combination in the frequency domain. We only need 4-9 parameters to reprent the diffuse lighting signal.

The rebuild implementation is as follows:

```
vec3 irradianceSH(vec3 n) {
 // uniform vec3 sphericalHarmonics[9] is 3 bands SH parameters
    return sphericalHarmonics[0]
    + sphericalHarmonics[1] * (n.y)
    + sphericalHarmonics[2] * (n.z)
    + sphericalHarmonics[3] * (n.x)
    + sphericalHarmonics[4] * (n.y * n.x)
    + sphericalHarmonics[5] * (n.y * n.z)
    + sphericalHarmonics[6] * (3.0 * n.z * n.z - 1.0)
    + sphericalHarmonics[7] * (n.z * n.x)
    + sphericalHarmonics[8] * (n.x * n.x - n.y * n.y);
}
```

We talk the Spherical Harmonics futher in latter blogs. 

## 3. IBL: Specular Part

### 3.1 Specular BRDF Integration

The specular part in rendering equation is as follows:

$$L_o(p, w_o) =  \int_0^Ω  k_s\frac{DFG}{4(w_o·n)(w_i·n)}L_i(p, w_i)n·w_idw_i$$

we simplify the BRDF part as one item.

$$L_o(p, w_o) =  \int_0^Ω  f_r(p, w_i, w_o)L_i(p, w_i)n·w_idw_i$$

As we can see, the BRDF $f_r$ part is not only related to the $w_i$ but also $w_o$, which increase the calculation cost significantly. To solve this proble, we apply the split sum approximation from Epic Games to split the specular part into two independent integral.

$$L_o(p, w_o) =  \int_0^ΩL_i(p, w_i)dw_i \int_0^Ω  f_r(p, w_i, w_o)n·w_idw_i $$


### 3.2 Pre-Filtered Environment Map


So, the first integration is called pre-filtered environment map, which is similar to the irradiance map. This item is related to the material roughness. As the roughness increases, there will be more blurry reflections.

$$\int_0^ΩL_i(p, w_i)dw_i $$

We store the different integration results related to different roughness in a cube map mipmap. 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/pfem.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/pfem.png" align="center"></a>
    <figcaption>the pre-filtered environment map.</figcaption>
</figure>

Since Specular requires the participation of BRDF, the normal distribution function needs to generate sampling neighbors and scattering intensity, which requires Normal and View as inputs.  When sampling the prefiltered environment map, the View direction is not known in advance, so in order to simplify the calculation, we assume v = n.

```
vec3 N = normalize(w_o);
vec3 R = N;
vec3 V = R;
```

The cost of the approximation is to destroy all view-dependent convolution effects, such as stretch reflection effects. This effect is acceptable. 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/cost.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/cost.png" align="center"></a>
    <figcaption>the approximation's influence.</figcaption>
</figure>

This approximation also destroys the effectiveness of the furnace test, which will result in a loss of energy. To compensate for the energy loss, we multiply the final result by a Scale Factor K to ensure that the average irradiance is correct. We will talk about the energy compensation in further blogs.

Be attention, **the reflection probe stores the prefiltered environment map**.

#### Improtance Sampling

Question: Typically, sampling is performed on a hemisphere to convolve the environment map. This method is feasible for the generation of irradiance map, but is poor for specular reflection. This is because specular reflection depends on the roughness of the surface. The reflected light may be loose or tight, but it must be around the reflection vector r.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ref.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ref.png" align="center"></a>
    <figcaption>reflection vector.</figcaption>
</figure>

Therefore, the specular lobes change with roughness: as the roughness increases, the specular lobes get larger. It is necessary to consider that most of the light will be reflected into a mirror lobe based on the half vector. **It makes sense to select samples in a similar way when sampling**, because the rest of the vectors are wasted. This is the importance of sampling in lighting.

We sample neighbors are generated only in certain areas that surround the half vector of the micro surface with roughness constraints. Combining quasi-Monte Carlo sampling with low-difference sequences and using importance sampling to bias samples can achieve **higher convergence speeds**.

#### GGX Importance Sampling

According to the roughness, the macroscopic reflection direction of the half vector of the micro surface is sampled. Starts a large loop that generates a random sequence of values that generates sample vectors in tangent space, transforms the samples into world space, and samples the radiance of the scene.

The following code generates 4096 sampling points. These sampling points can be stably reproduced and have a certain degree of randomness.

```
const uint SAMPLE_COUNT = 4096;
for(uint i = 0u; i < SAMPLE_COUNT; ++i)
{
    vec2 xi = Hammersley(i, SAMPLE_COUNT);
}
```

GGX Importance sampling samples the vector around the mirror lobes with roughness. 

```
vec3 ImportanceSampleGGX(vec2 xi, vec3 N, float roughness)
{
    float a = roughness * roughness;
    float phi = 2.0 * PI * xi.x;
    float cosTheta = sqrt((1.0 - xi.y) / (1.0 + (a * a - 1.0) * xi.y))
    float sinTheta = sqrt(1.0 - cosTheta * cosTheta);

    //from spherical coordinates to cartesian coordinates
    vec3 H;
    H.x = cos(phi) * sinTheta;
    H.y = sin(phi) * sinTheta;
    H.z = cosTheta;

    //tangent-space vector
    vec3 up = abs(N.z) < 0.999 ? vec3(0,0,1) : vec3(1, 0,0);
    vec3 tangent = normalize(cross(up, N));
    vec3 bitangent = cross(N, tangent);

    //from tangent-space vector to world-space sample vector
    vec3 sampleVec = tangent * H.x + bitangent * H.y + N * H.z;
    return normalize(sampleVec);
}
```

#### The Implementation of Prefiltered Environment Map

```
vec3 N = normalize(w_o);
vec3 R = N;
vec3 V = R;

const uint SAMPLE_COUNT = 1024;
float totalWeight = 0.0f;
vec3 prefilteredColor = vec3(0.0f);

for(uint i = 0u; i < SAMPLE_COUNT; ++i)
{
    vec2 xi = Hammersley(i, SAMPLE_COUNT);
    vec3 H = ImportanceSampleGGX(xi, N, roughness);
    vec3 L = normalize(2.0 * dot(V, H) * H - V);
    float NdotL = max(dot(N, L), 0.0);
    if(NdotL > 0.0){
        prefilteredColor += texture(environmentMap, L).rgb * NdotL;
        totalWeight += NdotL;
    }
}
prefilteredColor = prefilteredColor / totalWeight;
```

Be attention, the above convolution process is only for a target roughness and a single face of cubemap. That means, if we wanna complete the prefiltered-calculation, we should loop the above process for:
* maxMipLevels
* 6 cube maps

The prefiltered environment map is stored in the reflection probe.

Question: The specular reflection map always have **high frequency** detail, which will lead to the noisy points in prefiltered environment map.

Solution: 
* increase the sampling points
* we donot sample the environment map directly during pre-filtered convolution, but the mipmap of the environment map.

We calculate the mip map level based on the integral's PDF and roughness. The mip level of the environment map to be sampled is calculated as follows.

```
float D = DistributionGGX(NDotH, roughness);
float pdf = (D * NdotH / (4.0 * NdotV)) + 0.0001;
float resolution = 512.0; //resolution of source cubemap
float saTexel = 4.0 * PI / (6.0 * resolution * resolution);
float saSample = 1.0 / (SAMPLE_COUNT * pdf + 0.0001);
float mipmap = roughness == 0.0 ? 0.0 : 0.5 *log2(saSample / saTexel);
```

### 3.3 BRDF Integral

The second part of the integral is the specular BRDF part. To calculate the BRDF integral, assume that the incident irradiance in each direction is white, that is, 1.0f (here it is assumed that the prefiltered map is a white map).

$$ \int_0^Ω  f_r(p, w_i, w_o)n·w_idw_i$$

Epic stores the results of BRDF integration in a 2D LUT texture. The integral of BRDF is related to the two independent variables roughness and n*wi. Therefore, in the sampling of the BRDF integral texture, **the horizontal axis is the input n·wi of BRDF**, and **the vertical axis is roughness**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/lut.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/lut.png" align="center"></a>
    <figcaption>The Lut texture.</figcaption>
</figure>

The usage of LUT in shader:

```
 float lod = getMipLevelFromRoughness(roughness);
 vec3 prefilteredColor = textureCubeLod(PrefilteredEnvMap, refVec, lod);
 vec2 envBRDF = texture2D(BRDFIntegrationMap, vec2(NdotV, roughness)).xy;
 vec3 indirectSpecular = prefilteredColor * (F * envBRDF.x + envBRDF.y);
```

#### the BRDF Integral Analysis

We further divide the specular part integral based on the Fresnel item.

$$ \int_0^Ω  f_r(p, w_i, w_o)n·w_i dw_i = \int_0^Ω  f_r(p, w_i, w_o)\frac{F(w_o, h)}{F(w_o, h)}n·w_i dw_i$$


$$\int_0^Ω  \frac{f_r(p, w_i, w_o)}{F(w_o, h)}(F_0+(1-F_0)(1-w_o·h)^5)n·w_i dw_i$$

We assume the α is $(1-w_o·h)^5$. Then, we can divide the equation into two independent integral.

$$\int_0^Ω  \frac{f_r(p, w_i, w_o)}{F(w_o, h)}(F_0(1-α))n·w_i dw_i + \int_0^Ω  \frac{f_r(p, w_i, w_o)}{F(w_o, h)}(α)n·w_i dw_i$$

The two integrals represent the scale and bias of F0 respectively. Solve the above two integrals respectively and store the corresponding values in the BRDF integral map in the x, y channels. The BRDF convolution shader solves on the 2D plane, taking NdotV and roughness as input, according to the geometry of the BRDF function and Fresnel approximation to handle sampled vectors.

```
vec2 IntegrateBRDF(float NdotV, float roughness){
    vec3 V;
    V.x = sqrt(1.0 - NdotV*NdotV);
    V.y = 0.0;
    V.z = NdotV;

    float A = 0.0;
    float B = 0.0;

    vec3 N = vec3(0.0, 0.0, 1.0);

    const uint SAMPLE_COUNT = 1024u;
    for(uint i = 0u; i < SAMPLE_COUNT; ++i){
        vec2 Xi = Hammersley(i, SAMPLE_COUNT);
        vec3 H = ImportanceSampleGGX(Xi, N, roughness);
        vec3 L = normalize(2.0 * dot(V, H) * H - V);

        float NdotL = max(L.z, 0.0); 
        float NdotH = max(H.z, 0.0);
        float VdotH = max(dot(V, H), 0.0);
        if(NdotL > 0.0)
        {
            float G = GeometrySmith(N, V, L, roughness);
            float G_Vis = (G * VdotH) / (NdotH * NdotV);
            float Fc = pow(1.0 - VdotH, 5.0);

            A += (1.0 - Fc) * G_Vis;
            B += Fc * G_Vis;
        }
    }
    A /= float(SAMPLE_COUNT);
    B /= float(SAMPLE_COUNT);
    return vec2(A, B);
}

void main()
{
    vec2 integratedBRDF = IntegrateBRDF(TexCoords.x, TexCoords.y);
    FragColor = integratedBRDF;
}
```

Be attention, the geometry item have some difference between the PBR and BRDF integral calculation.

In PBR:

$$k_{direct}=\frac{(α+1)^2}{8}$$

$$k_{ibl}=\frac{α^2}{2}$$

## 4. IBL in shader

In this section, we discuss how the IBL apply in the real-time application. We assume that we have got the prefiltered environment map and BRDF LUT.

```
uniform samplerCube prefilterMap;
uniform sampler2D brdfLUT;

vec3 R = reflect(-V, N);
const float MAX_REFLECTION_LOD = 4.0
vec3 F = FresnelSchlickRoughness(max(dot(N, V), 0.0), F0, roughness);


vec3 kS = F;
vec3 kD = 1.0 - kS;
kD *= 1.0 - metallic;

//indirect diffuse
vec3 irradiance = texture(irradianceMap, N).rgb;
vec3 diffuse = irradiance * albedo;

//indirect specular
vec3 prefilteredColor = textureLod(prefilterMap, R, roughness *
MAX_REFLECTION_LOD).rgb;
vec2 envBRDF = texture(brdfLUT, vec2(max(dot(N, V), 0.0), roughness)).rg;
vec3 specular = prefilteredColor * (F * envBRDF.x + envBRDF.y);

//result with IBL
vec3 ambient = (kD * diffuse + specular) * ao;
```

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/demo.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/demo.png" align="center"></a>
    <figcaption>The IBL Demo.</figcaption>
</figure>


## 5. The Implementation in Modern Game Engine

In this chapter, we pay attention on the implementation of IBL in modern game engine.

We use the spherical harmonic to encode the diffuse environment lighting.

```
vec3 irradianceSH(vec3 n) {
 // uniform vec3 sphericalHarmonics[9] is 3 bands SH parameters
    return sphericalHarmonics[0]
    + sphericalHarmonics[1] * (n.y)
    + sphericalHarmonics[2] * (n.z)
    + sphericalHarmonics[3] * (n.x)
    + sphericalHarmonics[4] * (n.y * n.x)
    + sphericalHarmonics[5] * (n.y * n.z)
    + sphericalHarmonics[6] * (3.0 * n.z * n.z - 1.0)
    + sphericalHarmonics[7] * (n.z * n.x)
    + sphericalHarmonics[8] * (n.x * n.x - n.y * n.y);
}
```

Sample the brdf lut. The prefiltered environment map may store in a Octahedral map, so decodeEnvironmentMap function is used to decode the specular integral results.

In chapter 4 implementation, we assume the F90 is 1. In this implementation, we consider the F90 is not 1.

```
vec2 prefilteredDFG_LUT(float coord, float NoV) {
    // coord = sqrt(roughness), which is the mapping used by the
    // IBL prefiltering code when computing the mipmaps
    return textureLod(dfgLut, vec2(NoV, coord), 0.0).rg;
}

vec3 evaluateSpecularIBL(vec3 r, float perceptualRoughness) {
    // we assumes a 256x256 cubemap, with 9 mip levels
    float lod = 8.0 * perceptualRoughness;
    // decodeEnvironmentMap() either decodes RGBM or is a no-op if the
    // cubemap is stored in a float texture
    return decodeEnvironmentMap(textureCubeLodEXT(environmentMap, r, lod));
}

vec3 evaluateIBL(vec3 n, vec3 v, vec3 diffuseColor, vec3 f0, vec3 f90, float
perceptualRoughness){
    float NoV = max(dot(n, v), 0.0);
    vec3 r = reflect(-v, n);

    vec3 indirectSpecular = evaluateSpecularIBL(r, perceptualRoughness);
    vec2 env = prefilteredDFG_LUT(perceptualRoughness, NoV);
    vec3 specularColor = f0 * env.x + f90 * env.y;

    vec3 indirectDiffuse = max(irradianceSH(n), 0.0) * Fd_Lambert()
    return diffuseColor * indirectDiffuse + indirectSpecular * specularColor;
}
```

There is another method, we have no need to store the LUT texture. We can use the mathematics approximation function to simulate the LUT.

```
float3 EnvBRDFApprox( float3 SpecularColor, float Roughness, float NoV )
{
    // [ Lazarov 2013, "Getting More Physical in Call of Duty: Black Ops II" ]
    const float4 c0 = { -1, -0.0275, -0.572, 0.022 };
    const float4 c1 = { 1, 0.0425, 1.04, -0.04 };

    float4 r = Roughness * c0 + c1;
    float a004 = min( r.x * r.x, exp2( -9.28 * NoV ) ) * r.x + r.y;
    float2 AB = float2( -1.04, 1.04 ) * a004 + r.zw;

    return SpecularColor * AB.x + AB.y;
}
```

## 6. IBL for different shading model

### 6.1 Clear Coat

```
// clearCoat_NoV == shading_NoV if the clear coat layer doesn't have its own normal map
float Fc = F_Schlick(0.04, 1.0, clearCoat_NoV) * clearCoat;
// base layer attenuation for energy compensation
iblDiffuse *= 1.0 - Fc;
iblSpecular *= sq(1.0 - Fc);
 iblSpecular += specularIBL(r, clearCoatPerceptualRoughness) * Fc;
```

### 6.2 Anisotropy

This technique is from *Rendering the World of Far Cry 4* in GDC.

This technique replace the importance sampling with  the Bent Reflection Vector.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ani.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ani.png" align="center"></a>
    <figcaption>The Anisotropic.</figcaption>
</figure>


```
vec3 anisotropicDirection = anisotropy >= 0.0 ? bitangent : tangent;
vec3 anisotropicTangent = cross(anisotropicDirection, v);
vec3 anisotropicNormal = cross(anisotropicTangent, anisotropicDirection);
vec3 bentNormal = normalize(mix(n, anisotropicNormal, anisotropy));
vec3 r = reflect(-v, bentNormal);
```

### 6.3 Cloth

This technique from the paper *Production Friendly Microfacet Sheen BRDF*.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/cloth.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/cloth.png" align="center"></a>
    <figcaption>The cloth shading model LUT.</figcaption>
</figure>

```
float diffuse = Fd_Lambert() * ambientOcclusion;
#if defined(SHADING_MODEL_CLOTH)
#if defined(MATERIAL_HAS_SUBSURFACE_COLOR)
diffuse *= saturate((NoV + 0.5) / 2.25);
#endif
#endif

vec3 indirectDiffuse = irradianceIBL(n) * diffuse;
#if defined(SHADING_MODEL_CLOTH) && defined(MATERIAL_HAS_SUBSURFACE_COLOR)
indirectDiffuse *= saturate(subsurfaceColor + NoV);
#endif

vec3 ibl = diffuseColor * indirectDiffuse + indirectSpecular * specularColor;
```





