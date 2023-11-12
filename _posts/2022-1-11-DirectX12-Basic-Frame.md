---
layout: post
title:  "DirectX12 Graphics Objects"
date:   2022-1-12
excerpt: "The basic of the modern graphic API."
tag:
- Render 
- DirectX12
- Graphics
graphics: true
feature: https://github.com/OneSilverBullet/SilverGamer.GitHub.io/blob/gh-pages/_img/blogHead/directX12partI.jpg
---

## 1.Overview

The DirectX12 Initialize can be divided into several stages.
* Debug Layer
* Graphice Device
* Synchronization Fence
* MSAA Level Query
* Command Object
  * Command Queue
  * Command Allocator
  * Command List
* Swap Chain
  * Render Target View
  * Depth Stencil View

The relations between directX12 items is as following figure:

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/DataStructure.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/DataStructure.png" align="center"></a>
    <figcaption>DirectX12 Basic Data Structure.</figcaption>
</figure>

## 2. DX12 Objects

### 2.1 Basic Objects

##### swap chain

Swap Chain is the basic structure to switch over the framebuffer present to the player.
The swap chain is responsible for presenting the rendered image to the window.
The swap chain stores no less than 2 buffers. 
* IDXGISwapChain::Present function swasp the back buffer and the front buffer.

##### Create Command Queue

The command queue stores the commands which will send to GPU.
There could have multi command lists, but only one command queue.

- D3D12_COMMAND_LIST_TYPE_DIRECT: The command queue can be used to **execute draw, compute, and copy commands**. This is the most general type of command queue and will be used in most cases.
- D3D12_COMMAND_LIST_TYPE_COMPUTE: The command queue can be used to **execute compute and copy commands**.
- D3D12_COMMAND_LIST_TYPE_COPY: Command queue can be used to **execute copy commands**.


**Flush Function** is used to ensure any commands previously executed on the GPU.
- advisable to flush the GPU command queue before releasing any resources.


##### Command List & Allocator

A command list is used for recording commands that executed on the GPU. **Commands on a command list are not executed until the command list is sent to the command queue**.
Command List is the function set of the graphics operations. GPU commands are first recorded into a ID3D12GraphicsCommandList.

- D3D12_COMMAND_LIST_TYPE_DIRECT: Specifies a command buffer that the GPU can execute. A direct command list doesn’t inherit any GPU state.
- D3D12_COMMAND_LIST_TYPE_BUNDLE: Specifies a command buffer that can be executed only directly via a direct command list. A bundle command list inherits all GPU state (except for the currently set pipeline state object and primitive topology).
- D3D12_COMMAND_LIST_TYPE_COMPUTE: Specifies a command buffer for computing.
- D3D12_COMMAND_LIST_TYPE_COPY: Specifies a command buffer for copying.


The ID3D12CommandAllocator serves as the **backing memory for recording the GPU commands** into a command list. 

##### Render Target View

The back buffer textures of the swap chain are described using a render target view (RTV).

The **render target view describes the location of the texture resource in GPU memory**, the dimensions (width and height) of the texture, as well as the format of the texture.（A view in DirectX 12 is also called a descriptor.）

A render target view (RTV) describes a resource that can be attached to a bind slot of the output merger stage


##### Descriptor Heap


For the target of storing descriptor, we should build the heaps first. With the purpose of storing the render target and depth target, we should build the rtv, dsv heaps.

A descriptor heap can be visualized as **an array of descriptors (views)**. A view simply describes a resource that resides in GPU memory.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/dh.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/dh.png" align="center"></a>
    <figcaption>DirectX12 Descriptor Heap.</figcaption>
</figure>


A descriptor heap can be considered an array of resource views.
- Render Target Views RTV
- Shader Resource Views SRV
- Unordered Access Views UAV
- Constant Buffer Views CBV


The depth buffer requires a depth-stencil view(DSV). The DSV is stored in a descriptor heap. A descriptor heap is an array of descriptors(resource views).

##### IID_PPV_ARGS

IID_PPV_ARGS macro retrieve an interface pointer, supplying the IID value of the requested interface automatically based on the type of the interface pointer used.
- [in] parameter, normally of type REFIID, to specify the IID of the interface to retrieve.
- [out] paramteter, type void**, to receive the interface pointer.

##### Device

The device is the abstraction of your default GPU. The DirectX 12 device is **used to create resources (such as textures and buffers, command lists, command queues, fences, heaps, etc…)**. The DirectX 12 device is not directly used for issuing draw or dispatch commands. The DirectX 12 device can be considered **a memory context that tracks allocations in GPU memory**.

##### Fence

A Fence is an interface for a CPU/GPU synchronization object. The fence interal value is a 64bit unsigned integer.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/fence.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/fence.png" align="center"></a>
    <figcaption>The synchronization functions.</figcaption>
</figure>

The fence mechanism is as following figure. The fence divide the command queue to several part. Meanwhile, the fence notify CPU the process of the GPU in command queue. The fence is unique for a command queue, and CPU and GPU can both read/write it.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/fence2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/fence2.png" align="center"></a>
    <figcaption>The fence mechanism.</figcaption>
</figure>

The CPU call a Signal function in X_cpu, which will add a command at the end of the command queue in GPU. When GPU process this command, the fence value will become n+1. 

So the basic synchronization of CPU and GPU:

{% highlight cpp %}

	UINT64 mCurrentFence = 0;
	mCurrentFence++;
	ThrowIfFailed(mCommandQueue->Signal(mFence.Get(), mCurrentFence));

	if (mFence->GetCompletedValue() < mCurrentFence)
	{
		HANDLE eventHandle = CreateEventEx(nullptr, false, false, EVENT_ALL_ACCESS);
		ThrowIfFailed(mFence->SetEventOnCompletion(mCurrentFence, eventHandle));
		WaitForSingleObject(eventHandle, INFINITE);
		CloseHandle(eventHandle);
	}
	
{% endhighlight %}


### 2.2 Resource Objects

##### Resource

Resource: ID3D12Resource used to describe a buffer or a texture.

##### Resource Memory Manager

Memory Management: ID3D12Heap control the resource is stored in GPU memory, allows for various memory mapping techniques to be implemented.

There are several ways that GPU resources are allocated with a memory heap ID3D12Heap:
- Committed Resource
- Placed Resource
- Reserved Resource

##### Committed Resource

ID3D12Device::CreateCommittedResource: Creates both the resource and **an implicit heap** that is large enough to hold the resource.

Committed resources are **ideal for allocating large resource like textures or statically sized resources**. It is commonly used to create large resource in an upload heap.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/commit.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/commit.png" align="center"></a>
    <figcaption>The Committed Resource.</figcaption>
</figure>

##### Placed Resource

A placed resource is explicitly placed in a heap at a specific offset within the heap.
First, create a heap by ID3D12Device::CreateHeap, then create a placed heap in the heap by ID3D12Device::CreatePlacedResource.
- better performance
- implement various memory management techniques.


Depending on the GPU architecture, the resource type that we can allocate with a particular heap may be limited.
- ALLOW_ONLY_BUFFERS
- ALLOW_ONLY_RT_DS_TEXTURES
- ALLOW_ONLY_NON_RT_DS_TEXTURES
- ALLOW_ALL_BUFFERS_AND_TEXTURES

Multiple placed resources can be aliased in a heap as long as they donot access the same aliased heap space at the same time.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/ph.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/ph.png" align="center"></a>
    <figcaption>The Placed Resource.</figcaption>
</figure>

##### Reserved Resources

Reserved Resources can be created that are larger than can fit in a single heap. Portions of the reserved resource can be mapped using one or more heaps residing in physical GPU memory.

Reserved resources are created by ID3D12Device::CreateReservedResource. Before reserved resources are used, ID3D12CommandQueue::UpdateTileMappings is used to mapping a heap.

Using the reserved resources, **a large volume texture can be created using virtual memory but only the resident spaces of the volume texture needs to be mapped to physical memory**.
exe: Sparse Voxel Octree

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/rr.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/rr.png" align="center"></a>
    <figcaption>The Reserved Resource.</figcaption>
</figure>


##### Resource Barrier

Resources must be transitioned from one state to another using a resource barrier and inserting that resource barrier into the command list. Resource Barrier Type:
- Transition: transition a resourece to a particular state before using it.
- Aliasing: Specufies that a resource is used in a placed or reserved heap when that resource is aliaserd with other resource in the same heap.
- UAV: indicates that all UAV accesses to a particular resource have completed before any future UAV access can begin.
	*  Read > Write : Guarantees all previous read operations on the UAV have completed before being written to in another shader.
	* Write > Read
	* Write > Write


### 2.3 Pipeline Objects


##### Pipeline State Object

Pipeline State Object: provides the state management. Stage type:
- input assembly
- vertex shader
- hull shader
- domain shader
- geometry shader
- stream output
- rasterizer stage
- pixel shader 
- output merger

PSO contains most of the state that is required to configure the rendering pipeline.
- Shader Bytecode
- Vertex format input layout
- Primitive topology type
- Blende State
- Rasterizer State
- Depth-Strencil State
- Number of render targets and render target formats.
- Depth-Stencil format
- Multisample Description
- Stream output buffer description
- Root Signature

There are a few additional parameters that must be set outside of the pipeline state object.
- Vertex Index Buffer
- Stream output buffer
- Render targets
- Descriptor heaps
- Shader parameters
- Viewports
- Scissor Rectangle
- Constant blend factor
- Stencil reference value
- Primitive topology and adjacency information.

PSO can be specified for a graphics command list when the command list is reset by ID3D12GraphicsCommandList::Reset.
The command list can also change the PSO by ID3D12GraphicsCommandList::SetPipelineState.


##### Root Signature


Root Signature: **a root signature describe the parameters that are passed to the various programmable shader stages of the rendering pipeline**. texture samplers are also defined in the root signature.
- Root Arguments
- Parameters

Before the pipeline shader object (PSO) can be created, the root signature needs to be defined.

Shader Register & Register Spaces
- constant buffers are bound to b registers.
- shader resource views are bound to t registers.
- unorderd access views are bound to u registers.
- texture samplers are bound to s registers.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/shaderspace.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/shaderspace.png" align="center"></a>
    <figcaption>The Shader Space.</figcaption>
</figure>


A root signature can contain any number of parameters. Each parameter can be:
- D3D12_ROOT_PARAMETER_TYPE_32BIT_CONSTANTS
- D3D12_ROOT_PARAMETER_TYPE_CBV
- D3D12_ROOT_PARAMETER_TYPE_SRV
- D3D12_ROOT_PARAMETER_TYPE_UAV
- D3D12_ROOT_PARAMETER_TYPE_DESCRIPTOR_TABLE

**32-bit constants** : Constant buffer data can be passed to shader without the need to create a constant buffer resource by using the 32-bit constants.

**Inline Descriptpors**: Descriptors can be placed directly in the root signature without requiring a descriptor heap.
- Only  constants buffer and buffer resourecs: CBV, SRV, URV can be accessed using inline descriptors in the root signature.(32-bit value)

**Descriptor Tables**: A descriptor table defines several descriptor ranges that are placed consecutively in a GPU visible descriptor heap.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/dt.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/dt.png" align="center"></a>
    <figcaption>The descriptor table.</figcaption>
</figure>


Root Signature Constraints:
Root Signature are limited to  64 DWORDs. 
- 32-bit constants each costs 1 DWORD
- inline descriptors each costs 2 DWORD
- Descriptor tables each costs 1 DWORD
- Static Samples defines in the root signature do not count towards the size of the root signature.



The optimizer rule: Store the changeful Root Parameters in the TOP OF THE ROOT PARAMETERS HEAP.(RP Version Control)

**1.Create Root Parameters.**

Textures used in shader are initialized in the type of DESCRIPTOR_RANGE.


**2.Build Root Signature Descriptor**

The root signature descriptor is a data structure to describe the interface of the shader. The CD3DX12_ROOT_SIGNATURE_DESC need the followed data:

* the number of root parameters.
* slot root parameters.
* static samplers array data.
* static samplers array size.

**3.Create Root Signature**

Finnaly, we create the root signature by root signature descriptor. This will contain two steps:
* serialize the root signature descriptor.
* create root signature.



##### Load Resource

A normal process to load resources.

(1) Upload the vertex buffer data for a cube mesh.
- Create a vertex buffer view

(2) Upload the index buffer data for a cube mesh
- Create an index buffer view

(3) Create a descriptor heap for the depth-stencil view.

(4) Load the vertex shader and pixel shader.

(5) Create the input layout for the vertex shader.

(6) Create a root signature.

(7) Create a graphics pipeline state object(PSO).

(8) Create Depth Buffer. The depth buffer stores the depth of a pixel in normalized device coordinate space(NDC).


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/depth.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/depth.png" align="center"></a>
    <figcaption>The camera variables.</figcaption>
</figure>



## 3. Encapsulation Structure

### 3.1 Frame Resource

#### Conception

FrameResource: The resource needed to render the scene in one frame. 

FrameResource Array: While GPU process the n'th frame work, CPU will update the n+1'th frame resource. Loop array will make the GPU unlazy.

A FrameResource contains:
* commandListAllocator: store the commands associated with current frame.
* passCB: main pass constant buffer, store global constant values.
* materialCB: material constant buffer, store material values.
* objectCB: object constant buffer, store object transformation information.
* fence: check whether GPU is using current frame.


#### Implementation

The update logic:
* Exchange current frame resource.
* Waite GPU work.
* Update current frame resource data.

{% highlight cpp %}

	//Exchange current frame resource
	m_currentFrameResourceIndex = (m_currentFrameResourceIndex + 1) % 3;
	m_currentFrameResource = m_frameResources[m_currentFrameResourceIndex].get();

	//If GPU not finish the work, then waite
	if (m_currentFrameResource->m_fence && mFence->GetCompletedValue() < m_currentFrameResource->m_fence) {
		HANDLE eventHandle = CreateEventEx(nullptr, false, false, EVENT_ALL_ACCESS);
		ThrowIfFailed(mFence->SetEventOnCompletion(m_currentFrameResource->m_fence, eventHandle));
		WaitForSingleObject(eventHandle, INFINITE);
		CloseHandle(eventHandle);
	}

	// Update the constant buffer with the latest worldViewProj matrix.
	UpdateObjectCB(timer);
	UpdateMainPassCB(timer);
	UpdateMaterialCB(timer);
	UpdateCamera(timer);
{% endhighlight %}

Update the constant buffer in frameresource. For example, update the main pass constant buffer data in current frame resource.

{% highlight cpp %}
    XMMATRIX view = XMLoadFloat4x4(&mView);
	XMMATRIX proj = XMLoadFloat4x4(&mProj);
	XMMATRIX viewProj = XMMatrixMultiply(view, proj);
	XMMATRIX invView = XMMatrixInverse(&XMMatrixDeterminant(view), view);
	XMMATRIX invProj = XMMatrixInverse(&XMMatrixDeterminant(proj), proj);
	XMMATRIX invViewProj = XMMatrixInverse(&XMMatrixDeterminant(viewProj), viewProj);
	XMStoreFloat4x4(&m_mainPassConstants.m_view, XMMatrixTranspose(view));
	XMStoreFloat4x4(&m_mainPassConstants.m_proj, XMMatrixTranspose(proj));
	XMStoreFloat4x4(&m_mainPassConstants.m_viewProj, XMMatrixTranspose(viewProj));
	...

	auto currentPassCB = m_currentFrameResource->m_passCB.get();
	currentPassCB->CopyData(0, m_mainPassConstants);
{% endhighlight %}




