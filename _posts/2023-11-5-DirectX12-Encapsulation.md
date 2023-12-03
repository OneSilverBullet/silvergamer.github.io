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




## 2. Types

### The Map() function implementation 

ID3D12Resource::Map

Gets a **CPU pointer** to the specified subresource in the resource, but may not disclose the pointer value to applications. **Map also invalidates the CPU cache**, when necessary, so that CPU reads to this address reflect any modifications made by the GPU.


One way is to set up a virtual memory mapping that makes the resource's actual memory (could be system memory or GPU memory) visible as part of the application's address space. If it does happen to be GPU memory, any writes you do would typically be buffered up and sent across the PCIe bus to the GPU, using write combining. (Even on a unified-memory system, mapped resource memory probably has some different caching behavior from "regular" memory, since CPU and GPU caches aren't coherent with each other.)

The other way the driver can implement Map() is to allocate memory and create a local copy of the resource, let your app edit it, then copy the modified memory back to the resource when Unmap() is called. This might need to be done if the actual memory layout of the resource on the GPU is "weird" somehow, like having a nonstandard tiling scheme or some such.

The copying approach is clearly more expensive due to the extra copies required, so I'd expect the driver to use the memory mapping approach whenever possible.



## 3. Dynamic Descriptor Heap

### 3.1 

The static members for all classes
* num descriptors per heap: 1024
* mutex 
* descriptor heap pool[vector][2]: all of the descriptor heaps
* retired descriptor heap[queue][2]: the descriptor heap which retired.
* available descriptor heap[queue][2]: the descriptor heap available for allocating descriptor.

The Static Methods

(1) Request Descriptor Heap

There is two dynamic descriptor heap type: D3D12_DESCRIPTOR_HEAP_TYPE_SAMPLER and D3D12_DESCRIPTOR_HEAP_TYPE_CBV_SRV_UAV. According to the dynamic descriptor heap type to choose which heap pool to use.

Steps:

a. Check the retired descriptor heaps to find whether there is descriptor heap with fence completed. If there are free descriptor heaps, put these descriptor heaps to  the avaliable descriptor heaps queue.

b. If there is no descriptor heap with completed fence, then create a descriptor heap with flag D3D12_DESCRIPTOR_HEAP_FLAG_SHADER_VISIBLE and push the heap to the available descriptor heaps queue. 

c. If there is descriptor heap in available heaps, return the front item in available descriptor heap.

(2) Discard Descriptor Heaps

This function has two input:
* fence value: After completing the fence, the used descriptor heaps is useless.
* descriptor heaps vector: the used descriptor heaps vector.

Steps:

a. Push all the used descriptor heaps into the retired descriptor heaps. These descriptor heaps are full of descriptors.


The essential logic of dynamic descriptor heap:

DynamicDescriptorHeap::DescriptorHandleCache::CopyAndBindStaleTables





Descriptor Table Cache: Describes a descriptor table entry, a region of the handle cache and which handles have been set.
* assigned handles bit map: uint32_t
* table start: D3D12_CPU_DESCRIPTOR_HANDLE*
* table size: uint32_t

Descriptor Handle Cache


The root signature is the **empty slots** to interpret the shader parameters. 

The descriptor heap is responsible for **the assignment of shader parameters** corresponding to **the root signature**.

The mechanism of dynamic descriptor heap:

(1) Parse Root Signature

parse the root signature and create related descriptor table structure.











