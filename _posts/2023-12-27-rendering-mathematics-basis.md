---
layout: post
title:  "The Mathematics behind the Image based Rendering"
date:   2023-12-25
excerpt: "The common mathematics methods in rendering."
tag:
- Computer Graphics 
graphics: true
feature: https://github.com/OneSilverBullet/SilverGamer.GitHub.io/blob/gh-pages/_img/graph/head.png

---

## 1. Monte Carlo Mathematics Basis

In this chapter, we will discuss some common mathematics tools about rendering.

### 1.1 Random Variable

Firstly, we should understand two essential conception in probability theory, which play a important role in off-line rendering.
* Probability Density Function(PDF): The probability distribution function composed of all possible values of a random variable. Random variables can be **continuous** and **discrete**. The sum of all probabilities is 1. p(x) Function.  
* Cumulative Distribution Function(CDF): P(x) Function.

$$P(y) = Pr(x≤y)=\int_{-∞}^y p(x)dx$$

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/fd.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/fd.png" align="center"></a>
    <figcaption>the function diagram.</figcaption>
</figure>

CDF can be exchanged to the integral of PDF.

$$Pr\{a≤x≤b\}=Pr\{x≤b\} - Pr\{x≤a\}$$

$$ Pr\{a≤x≤b\}= P(b) - P(a) = \int_{a}^b p(x)dx$$

### 1.2 Expectation and Variance

**(1) Expectation**

As for discrete random variable X, we can assume that the sample posibility is $p_i$. The expectation of random variable X is as follows:

$$E[X] = \sum_{i=1}^{n}p_ix_i$$

As for continuous random variable X, the expectation is as follows:


$$E[X] = \int_{-∞}^{∞}xp(x)dx$$

**(2) Law of the unconscious statistician**

When we know the distribution of the random variable X and unknow the distribution of g(x), we can apply the Law of the unconcious statistician.

When X is a discrete random variable:

$$E[Y] = E[g(X)] = \sum_{i=1}^{n}p_ig(x_i)$$

When X is a continuous random variable:

$$E[Y] = E[g(X)] = \int_{-∞}^{∞}g(x)p(x)dx$$

**(3) Variance**

Variance is a representation of the difference between the expectation and the sampling results, which can be calculated by the absolute expectation of subtraction between sampling results and expectation.

As for discrete random variable:

$$ σ^2 =  E[(x - E[X])^2] = \sum_{i}(x_i - E[x])^2$$

As for continuous random variable:

$$ σ^2 =  E[(x - E[X])^2] = \int(x - E[x])^2p(x)dx$$

There are some properties for variance.

If C is a constant value, then:

$$V\{C\} = 0$$

If X is a random variable, and C is a constant value, then:

$$V\{CX\} = C^2V\{X\}$$

If X and Y are independent random variables, then:

$$V\{X+Y\} = V\{X\} + V\{Y\}$$

### 1.3 Law of Large Numbers

The pre-condition of the Law of Large Numbers is that every sample is **Independent Identically Distributed**, which means every samples have the same probability density function p.

The Law of Large Numbers: the statistics average value of random variable is converged to the expectation.

$$P\{E[X] = \lim_{N→∞}\frac{1}{N}\sum_{i=1}^{N}x_i\} = 1$$



## 2.  Monte Carlo Method

### 2.1 Conception

The Monte Carlo helps us in discretely solving the problem of figuring out some statistic or value of a population without having to take all of the population in consider.

The basic Monte Carlo Estimation:

$$I = \int_a^bf(x)dx$$

$$I≈\sum_{i=1}^Nf(x_i)\frac{b-a}{N}$$


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/monte.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/monte.png" align="center"></a>
    <figcaption>the monte carlo estimation.</figcaption>
</figure>

Furthermore, we can get the Monte Carlo Integral:

$$E[f(x)] = \int_{S} f(x)p(x)dx ≈ \frac{1}{N}\sum_{i=1}^{N}f(x_i)$$

The props of Monte Carlo Integral:
* It is suitable for multi-dimensional integral computation.
* Unbiased and consistent.
* It is suitable for parallel.

The cons of Monte Carlo Integral:
* It is hard to convergence.
* It is hard to estimate the variance of the function f(x).

### 2.2 Monte Carlo Method's Properties

(1) The samples number incerase, the more the Monte Carlo Estimator converges to the real value.

$$P\{\lim_{N→∞}<F^N>=F(X)\} = 1$$

(2) The speed of convergence is in direct proportion to the variance value.

(3) The Central Limit Theorem states the asymptotic distribution of the Monte Carlo Estimator. 

In rendering, the disadvantages of Monte Carlo Integral is hard to convergence because of big variance, which leads to noise points in  rendering results. 

To solve the problem, we should pay attention to reduce the variance of function:
* Importance Sampling
* Quasi Monte Carlo

### 2.3 Inverse Conversion Method

We assume that X is a continuous variable, the accumulate distribute function is $P_X$. If random variable Y is a uniform distribution in [0, 1], the random variable $P_X(Y)^{-1}$ has the same probability distribution with X.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ic.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ic.png" align="center"></a>
    <figcaption>the inverse conversion method.</figcaption>
</figure>

The dy is the uniform distribution in [0, 1]. So, the probability P(x) of random variable X in dx can be calculated in follows:

$$\frac{dP(x)}{dx} = \frac{dy}{dx}$$

The uniform distribution of Y can be mapped to the random variable satisfied the P(x).

Above all, the inverse conversion sampling methods is as follows:
* Calculate the accumulate distribution function of p(x).
* calculate the inverse function of the accumulate distribution function.
* generate a random value α in the uniform distribution around [0, 1].
* Put the random number α into the inverse function, and we can get the random variable satisfied p(x).



### 2.4 Variance Reduction Methods: Importance Sampling

As shown in the figure below, only a few directions of light contribute to the rendering of P. So if we abandon uniform sampling, but choose importance sampling, the effect will be very different.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/is.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/is.png" align="center"></a>
    <figcaption>the importance sampling.</figcaption>
</figure>

To get the radiance of the point P, we should calculate the hemisphere integral of the rendering equation in P, which can be written in Monte Carlo type as follows.

$$L≈\frac{2π}{N}\sum_{i=0}^{N-1}L_i(w_i)$$

where the $w_i$ is the input direction of Light in hemisphere.

We have discussed about the rendering equation in the Image based Rendering blog, and the rendering equation is so complicate to calculate integral directly. The rendering equation always has a high  variance, which means it's hard to convergence by uniform sampling Monte Carlo Method.

So we should apply importance sampling to reduce the variance of rendering equation, and speed up the convergence process. We should consider two key points:
* The Monte Carlo Method is random and unbiased. If we changes the samples, the results will be meaningless in mathematics.
* The results may be over fit the samples.

If we reduce the variance of rendering equation, the number of valid sampling points is also greatly reduced.

Considering the variance of a constant function is 0, we can apply the calculus thinking to the integral process, which means that we can exchange the non-constant function to a constant function in micro view. There are two essential key points:
* If we divide a function by itself or a function proportional to it, we will get a constant.
* If we construct a probability density function f(x) which is directly proportional to the F(x), the Monte Carlo Estimator can get more accurate results with less samples.

So the integral problem of Monte Carlo Estimator can be changed to how to find a probability density function, which is close to the inetgral of F(x). In this way, we can reduce the variance of the function significantly.

$$<F^N> = \frac{1}{N}\sum_{i=0}^{N-1}\frac{f(x)}{pdf(x)}$$


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/is2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/is2.png" align="center"></a>
    <figcaption>the importance sampling.</figcaption>
</figure>


To be attention, if we cannot find a appropriate pdf, we should use the uniform sampling method. The bad pdf may be counterproductive.


### 2.5 Variance Reduction Methods: Quasi Monte Carlo

No matter the origin Monte Carlo Method or the Importance Sampling, we always meet the problem of Sample Clumping.
The Sample Clumping problem means that the samples are too close with each other, which leads to a bad sample results.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/sc.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/sc.png" align="center"></a>
    <figcaption>the sample clumping.</figcaption>
</figure>

The sample strategies have two categories:
* there is fix internal between each samples.
* there is totally random internal between each samples.

The idea of Quasi Monte Carlo method is to distribute the Sampling points between the above two cases, this sampling method is Stratified Sampling.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/qm.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/qm.png" align="center"></a>
    <figcaption>the quasi monte carlo sampling.</figcaption>
</figure>

The first line is totally random; the second line is uniform with fix internal; the third line is quasi monte carlo method.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ss.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/ss.png" align="center"></a>
    <figcaption>the stratified sampling.</figcaption>
</figure>


The Stratified Sampling: the internal function can be devided into several internals. We jitter the internal center to get better samples distribution.

$$<F^N> = \frac{b-a}{N}\sum_{i=0}^{N-1}f(a + \frac{i+ ξ }{N}(b-a))$$

$$-\frac{h}{2} ≤ ξ ≤  \frac{h}{2}$$

where h is the internal size. 

In this method, we can get a series of greate low-discrepancy sequences, such as Hammersley Sequence. When the low-discrepancy sequences are applied in monte carlo method, the rendering method is called quasi monte carlo.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/qm2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/qm2.png" align="center"></a>
    <figcaption>the quasi monte carlo ray tracing.</figcaption>
</figure>

Pros:
* Quasi monte carlo method has faster convergence speed.
* The rendering has less noisy and better effect.

Cons:
* Because the low-discrepancy sequences has periodical structure, which will lead to a discernible pattern.
    * We can apply a trick of rotating, which is usually used in SSAO.

## 3. Review the Image based Rendering

In the last blog, we only talk about the technical process of image based rendering. In this chapter, we will discuss about the mathematics theory behind the LUT generation process.

We consider the brdf integral part in the split rendering equation again, the equation is as follows. We further divide the specular part integral based on the Fresnel item.

$$ \int_0^Ω  f_r(p, w_i, w_o)n·w_i dw_i = \int_0^Ω  f_r(p, w_i, w_o)\frac{F(w_o, h)}{F(w_o, h)}n·w_i dw_i$$


$$\int_0^Ω  \frac{f_r(p, w_i, w_o)}{F(w_o, h)}(F_0+(1-F_0)(1-w_o·h)^5)n·w_i dw_i$$

We assume the α is $(1-w_o·h)^5$. Then, we can divide the equation into two independent integral.

$$\int_0^Ω  \frac{f_r(p, w_i, w_o)}{F(w_o, h)}(F_0(1-α))n·w_i dw_i + \int_0^Ω  \frac{f_r(p, w_i, w_o)}{F(w_o, h)}(α)n·w_i dw_i$$

The BRDF function is 

$$\frac{DFG}{4(w_o·n)(w_i·n)}$$

### 3.1 Importance Sampling Practice in IBL

We use the GGX Normal Distribution Function to generate the Importance Samples. We should use the **Inverse Conversion Method** in this process.

$$NDF-GGX(n, h, α)=\frac{α^2}{π(cosθ^2(α^2-1)+1)^2}$$

Then we exchange the GGX Normal Distribution Function to the polar coordinates type.

$$p(θ, φ) = \frac{α^2cosθsinθ}{π(cosθ^2(α^2-1)+1)^2}$$

Based on the polar coordinate type NDF, we calculate the edge probability density function of θ and φ.

$$p_h(θ)=\int_0^{2π}p_h(θ, φ)dφ = \frac{2α^2cosθsinθ}{π((α^2-1)cos^2θ+1)^2}$$

$$p_h(φ)=\int_0^{2π}p_h(θ, φ)dθ = \frac{1}{2π}$$

Furthermore, we get the cumulate distrbution function of θ and φ. 

$$P_h(φ)=\frac{φ}{2π}$$

$$P_h(θ) = \int_{0}^{θ}\frac{2α^2costsint}{π((α^2-1)cos^2t+1)^2}dt = \frac{α^2}{(α^2-1)^2 cos^2θ+(α^2-1)}-\frac{1}{α^2-1}$$

We assume the two random variable ξ and ϵ  in [0, 1] replace the two cumulate distrbution functions P_h(φ) and P_h(θ) separately. And we apply the **inverse conversion methods** to get the φ and θ value.

$$θ = arccos\sqrt{\frac{1-ϵ}{ϵ(α^2-1)+1}}$$

$$φ = 2πξ$$

In the above process, we exchange the GGX Normal Distribution Function to the polar coordinates φ and θ generation functions, which means we can map the [0, 1] variable to the GGX Normal Distribution. In unreal 4, we can implement the importance sampling as follows. 

The xi is generated by low-discrepancy sequences, which is a random variable between 0 and 1. We transfer the random variable to the importance samples with the GGX normal distribution. Finally, we transfer the polar coordinates in tangent space to the cartesian coordinates in world space.

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

### 3.2 BRDF LUT Generation Process

We can see that the GGXImportanceSamples function generates micro half vectors. As the figure shows, we use the importance sampling methods to generate 1024 micro half vectors. Each micro half vector is normal vector in a micro face. Then we will calculate the reflect vector based on the micro half vector and view vector. We will calculate the influence rate of current importance samples and cumulate the influence to the two parameters A and B.

One call of the integrateBRDF indicate one pixel generation process in BRDF LUT texture.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/brdfInt.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/graph/brdfInt.png" align="center"></a>
    <figcaption>the brdf integral process.</figcaption>
</figure>


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



