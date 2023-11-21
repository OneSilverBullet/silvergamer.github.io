---
layout: post
title:  "DirectX12 Encapsulations"
date:   2022-1-12
excerpt: "The encapsulation of the modern graphic API."
tag:
- Render 
- DirectX12
- Graphics
graphics: true
feature: https://github.com/OneSilverBullet/SilverGamer.GitHub.io/blob/gh-pages/_img/blogHead/directX12partI.jpg
---

## 1. Managing Descriptor Heaps

### 1.1 Introduction

Key Conceptions:
* A Descriptor: is a small block of data that **fully describes an object to the GPU, in a GPU specific opaque format**.
* Descriptor Heap: an array of descriptor.

Every pipeline state incorporates a **root signature**.
* root signature: defines how shader registers are mapped to the descriptors in the bound descriptor heaps.

Resource Binding Stages:

(1) Shader register is first mapped to the descriptor in a descriptor heap as defined by the root signature. 

(2) The descriptor (which may be SRV, UAV, CBV or Sampler) then references the resource in GPU memory.

There are four types of descriptor heaps:
* D3D12_DESCRIPTOR_HEAP_TYPE_CBV_SRV_UAV
* D3D12_DESCRIPTOR_HEAP_TYPE_SAMPLER
* D3D12_DESCRIPTOR_HEAP_TYPE_RTV
* D3D12_DESCRIPTOR_HEAP_TYPE_DSV

For the GPU to access descriptor in the heap, the heap must be shader-visible.


### 1.2 Overview





<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/DataStructure.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/DataStructure.png" align="center"></a>
    <figcaption>DirectX12 Basic Data Structure.</figcaption>
</figure>

## 1. Types

### .1 Parameter Passing

For efficiency purpose, XMVECTOR values can be passed as arguments to functions in **SSE/SSE2** registers instead of on the stack. We use following types to transfer XMVECTOR:
* FXMVECTOR
* GXMVECTOR
* HXMVECTOR
* CXMVECTOR

(1) The first three XMVECTOR parameters should be of type FXMVECTOR;

(2) The fourth XMVECTOR should be of type GXMVECTOR;

(3) The fifth and sixth XMVECTOR parameter should be of type HXMVECTOR;

(4) Any additional XMVECTOR parameters should be of type CXMVECTOR. 

Furthermore, the calling convention annotation XM_CALLCONV must be specified before the function name so that the proper calling
convention is used, which again depends on the compiler version.

```
inline XMMATRIX XM_CALLCONV XMMatrixTransformation(
 FXMVECTOR ScalingOrigin,
 FXMVECTOR ScalingOrientationQuaternion, .
 FXMVECTOR Scaling,
 GXMVECTOR RotationOrigin,
 HXMVECTOR RotationQuaternion,
 HXMVECTOR Translation);
```

