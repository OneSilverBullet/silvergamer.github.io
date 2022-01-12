---
layout: post
title:  "DirectX12 Basic Frame Work part I"
date:   2022-1-12
excerpt: "The first step of the modern graphic API."
tag:
- Render 
- DirectX12
- Graphics
comments: true
blog: true
feature: https://github.com/OneSilverBullet/SilverGamer.GitHub.io/blob/gh-pages/_img/blogHead/directX12partI.jpg
---

## Overview

* Initialize window.
* Initialize the graphics.
* Window On Resize.
* Message Process.

## Initialize Window

The stage of the construction of the window exe program.

* Regist WNDCLASS
{% highlight cpp %}
WNDCLASS wc;
	wc.style = CS_HREDRAW | CS_VREDRAW;
	wc.lpfnWndProc = MainWndProc;
	wc.cbClsExtra = 0;
	wc.cbWndExtra = 0;
	wc.hInstance = m_appInstance;
	wc.hIcon = LoadIcon(0, IDI_APPLICATION);
	wc.hCursor = LoadCursor(0, IDC_ARROW);
	wc.hbrBackground = (HBRUSH)GetStockObject(NULL_BRUSH);
	wc.lpszMenuName = 0;
	wc.lpszClassName = L"SilverGamer";
	if (!RegisterClass(&wc))
	{
		MessageBox(0, L"RegisterClass Failed.", 0, 0);
		return false;
	}
{% endhighlight %}

* Regulate the size of window.

{% highlight cpp %}
	RECT R = { 0, 0, m_appConfig.GetWidth(), m_appConfig.GetHeight() };
	AdjustWindowRect(&R, WS_OVERLAPPEDWINDOW, false);
{% endhighlight %}

* Create Window

{% highlight cpp %}
	m_mainWnd = CreateWindow(L"SilverGamer", m_appConfig.GetMainWndCaption().c_str(),
		WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, CW_USEDEFAULT,
		width, height, 0, 0, m_appInstance, 0);
{% endhighlight %}


* Update and Show the window

{% highlight cpp %}
	ShowWindow(m_mainWnd, SW_SHOW);
	UpdateWindow(m_mainWnd);
{% endhighlight %}

{% highlight cpp %}
 fragColor.rgb =
    mix
      ( fragColor.rgb
      , cmx.rgb
      , smoothstep(minThreshold, maxThreshold, mx)
      );
{% endhighlight %}
<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/DataStructure.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/DataStructure.png" align="center"></a>
    <figcaption>DirectX12 Basic Data Structure.</figcaption>
</figure>

## Initialize Graphics

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




##### Debug Layer

We can active the d3d debug layer in the program. The d3d12 API is encapsulation, and we can not trace the information behand the API. 
Activating the debug layer is the key to fix bugs in a d3d12 program.
We can catch the error information in the output window of Visual Studio.


{% highlight cpp %}
#if defined(DEBUG) || defined(_DEBUG) 
	// Enable the D3D12 debug layer.
	{
		Microsoft::WRL::ComPtr<ID3D12Debug> debugController;
		ThrowIfFailed(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController)));
		debugController->EnableDebugLayer();
	}
#endif
{% endhighlight %}

##### D3D12 Factory

Factory is the basis of the d3d12 program. The mother of all components you need.

{% highlight cpp %}
ThrowIfFailed(CreateDXGIFactory1(IID_PPV_ARGS(&mdxgiFactory)));
{% endhighlight %}

##### D3D12 Device

The device is the abstraction of your default GPU.

{% highlight cpp %}
	HRESULT hardwareResult = D3D12CreateDevice(
		nullptr,             // default adapter
		D3D_FEATURE_LEVEL_11_0,
		IID_PPV_ARGS(&md3dDevice));
{% endhighlight %}


##### Create Fence

The fence is a tool that keeps synchronization between GPU and CPU.

{% highlight cpp %}
ThrowIfFailed(md3dDevice->CreateFence(0, D3D12_FENCE_FLAG_NONE,
		IID_PPV_ARGS(&mFence)));
{% endhighlight %}

##### Create Command Queue

The command queue stores the commands which will send to GPU.
There could have multi command lists, but only one command queue.

{% highlight cpp %}
	D3D12_COMMAND_QUEUE_DESC queueDesc = {};
	queueDesc.Type = D3D12_COMMAND_LIST_TYPE_DIRECT;
	queueDesc.Flags = D3D12_COMMAND_QUEUE_FLAG_NONE;
	ThrowIfFailed(md3dDevice->CreateCommandQueue(&queueDesc, IID_PPV_ARGS(&mCommandQueue)));
{% endhighlight %}


##### Create Command Allocator

Command Allocator is the conatiner of the command list.

{% highlight cpp %}
	ThrowIfFailed(md3dDevice->CreateCommandAllocator(
		D3D12_COMMAND_LIST_TYPE_DIRECT,
		IID_PPV_ARGS(mDirectCmdListAlloc.GetAddressOf())));
{% endhighlight %}

##### Create Command List

Command List is the function set of the graphics operations.

{% highlight cpp %}
ThrowIfFailed(md3dDevice->CreateCommandList(
		0,
		D3D12_COMMAND_LIST_TYPE_DIRECT,
		mDirectCmdListAlloc.Get(), // Associated command allocator
		nullptr,                   // Initial PipelineStateObject
		IID_PPV_ARGS(mCommandList.GetAddressOf())));
{% endhighlight %}

##### Create Swap Chain

Swap Chain is the basic structure to switch over the framebuffer present to the player.

{% highlight cpp %}
	mSwapChain.Reset();

	DXGI_SWAP_CHAIN_DESC sd;
	sd.BufferDesc.Width = mClientWidth;
	sd.BufferDesc.Height = d
	
	d.BufferDesc.RefreshRate.Numerator = 60;
	sd.BufferDesc.RefreshRate.Denominator = 1;
	sd.BufferDesc.Format = mBackBufferFormat;
	sd.BufferDesc.ScanlineOrdering = DXGI_MODE_SCANLINE_ORDER_UNSPECIFIED;
	sd.BufferDesc.Scaling = DXGI_MODE_SCALING_UNSPECIFIED;
	sd.SampleDesc.Count = m4xMsaaState ? 4 : 1;
	sd.SampleDesc.Quality = m4xMsaaState ? (m4xMsaaQuality - 1) : 0;
	sd.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
	sd.BufferCount = SwapChainBufferCount;
	sd.OutputWindow = m_mainWnd;
	sd.Windowed = true;
	sd.SwapEffect = DXGI_SWAP_EFFECT_FLIP_DISCARD;
	sd.Flags = DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH;

	// Note: Swap chain uses queue to perform flush.
	ThrowIfFailed(mdxgiFactory->CreateSwapChain(
		mCommandQueue.Get(),
		&sd,
		mSwapChain.GetAddressOf()));
{% endhighlight %}

##### Create Descriptor Heap

For the target of storing descriptor, we should build the heaps first. With the purpose of storing the render target and depth target, we should build the rtv, dsv heaps.

{% highlight cpp %}
D3D12_DESCRIPTOR_HEAP_DESC rtvHeapDesc;
	rtvHeapDesc.NumDescriptors = SwapChainBufferCount;
	rtvHeapDesc.Type = D3D12_DESCRIPTOR_HEAP_TYPE_RTV;
	rtvHeapDesc.Flags = D3D12_DESCRIPTOR_HEAP_FLAG_NONE;
	rtvHeapDesc.NodeMask = 0;
	ThrowIfFailed(md3dDevice->CreateDescriptorHeap(
		&rtvHeapDesc, IID_PPV_ARGS(mRtvHeap.GetAddressOf())));

	D3D12_DESCRIPTOR_HEAP_DESC dsvHeapDesc;
	dsvHeapDesc.NumDescriptors = 1;
	dsvHeapDesc.Type = D3D12_DESCRIPTOR_HEAP_TYPE_DSV;
	dsvHeapDesc.Flags = D3D12_DESCRIPTOR_HEAP_FLAG_NONE;
	dsvHeapDesc.NodeMask = 0;
	ThrowIfFailed(md3dDevice->CreateDescriptorHeap(
		&dsvHeapDesc, IID_PPV_ARGS(mDsvHeap.GetAddressOf())));
{% endhighlight %}

### Create Descriptor Heap



## Window On Resize

