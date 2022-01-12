---
layout: post
title:  "DirectX12 Basic Frame Work part II"
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

## Shader Resource Update

### RootSignature

#### Conception

Root Signature: The layout structure of shader accessible resources. 

Root Parameter: Every Shader accessible data in Root Signature. Root Parameter's type:

* Root Constant: 32bit aligned constant data in RP. Access cost is 0.
* Root Descriptor: The Descriptor which mapping with a resource or a resource array. Access cost is 1.
* Descriptor Table: The Descriptor Table store in RP. Access cost is 2.


The optimizer rule: Store the changeful Root Parameters in the TOP OF THE ROOT PARAMETERS HEAP.(RP Version Control)

#### Implementation

##### 1.Create Root Parameters.

Textures used in shader are initialized in the type of DESCRIPTOR_RANGE.

{% highlight cpp %}
CD3DX12_DESCRIPTOR_RANGE texTable;
texTable.Init(D3D12_DESCRIPTOR_RANGE_TYPE_SRV, 1, 0);
{% endhighlight %}

And then, create the other buffers used in shader. for example, the constant buffers of main pass, render items, materials... These buffers is initialized as a constant buffer view.

{% highlight cpp %}
CD3DX12_ROOT_PARAMETER slotRootParameter[4];

	//create shader visible resource function param descriptor
slotRootParameter[0].InitAsDescriptorTable(1, &texTable, D3D12_SHADER_VISIBILITY_PIXEL);
slotRootParameter[1].InitAsConstantBufferView(0);
slotRootParameter[2].InitAsConstantBufferView(1);
slotRootParameter[3].InitAsConstantBufferView(2);
{% endhighlight %}

##### 2.Build Root Signature Descriptor

The root signature descriptor is a data structure to describe the interface of the shader. The CD3DX12_ROOT_SIGNATURE_DESC need the followed data:

* the number of root parameters.
* slot root parameters.
* static samplers array data.
* static samplers array size.

{% highlight cpp %}

//the shader samplers array
auto staticSamplers
GetStaticSamplers(); //Get the textures samplers

int samplerSize = staticSamplers.size();

//A root signature is an array of root parameters
CD3DX12_ROOT_SIGNATURE_DESC rootSigDesc(4, slotRootParameter, 
		(UINT)staticSamplers.size(),
		staticSamplers.data(),
		D3D12_ROOT_SIGNATURE_FLAG_ALLOW_INPUT_ASSEMBLER_INPUT_LAYOUT);
{% endhighlight %}

The GetStaticSamplers() function serves several types of texture samples.

{% highlight cpp %}
std::array<const CD3DX12_STATIC_SAMPLER_DESC, 6> BoxApplication::GetStaticSamplers()
{
	const CD3DX12_STATIC_SAMPLER_DESC pointWrap(
		0,
		D3D12_FILTER_MIN_MAG_MIP_POINT,
		D3D12_TEXTURE_ADDRESS_MODE_WRAP,
		D3D12_TEXTURE_ADDRESS_MODE_WRAP,
		D3D12_TEXTURE_ADDRESS_MODE_WRAP
	);
	
	...
{% endhighlight %}


##### 3.Create Root Signature

Finnaly, we create the root signature by root signature descriptor. This will contain two steps:
* serialize the root signature descriptor.
* create root signature.

{% highlight cpp %}
Microsoft::WRL::ComPtr<ID3D12RootSignature> mRootSignature = nullptr;
...
//serialize the root signature descriptor.
Microsoft::WRL::ComPtr<ID3DBlob> serializedRootSig = nullptr;
	Microsoft::WRL::ComPtr<ID3DBlob> errorBlob = nullptr;
	HRESULT hr = D3D12SerializeRootSignature(&rootSigDesc, D3D_ROOT_SIGNATURE_VERSION_1,
		serializedRootSig.GetAddressOf(), errorBlob.GetAddressOf());

//create root signature.
ThrowIfFailed(md3dDevice->CreateRootSignature(
		0,
		serializedRootSig->GetBufferPointer(),
		serializedRootSig->GetBufferSize(),
		IID_PPV_ARGS(&mRootSignature)));
{% endhighlight %}

And the root signature will used in commanlist and pso initialization.

{% highlight cpp %}
// command list setting
mCommandList->SetGraphicsRootSignature(mRootSignature.Get());

...

// pso init
D3D12_GRAPHICS_PIPELINE_STATE_DESC psoDesc;
	ZeroMemory(&psoDesc, sizeof(D3D12_GRAPHICS_PIPELINE_STATE_DESC));
	psoDesc.InputLayout = { mInputLayout.data(), (UINT)mInputLayout.size() };
	psoDesc.pRootSignature = mRootSignature.Get();
{% endhighlight %}


### Upload Buffer

#### Conception

Upload buffer encapsulate the functions and buffer between CPU and GPU.


#### Implementation

The Structure Interface.
* m_uploadBuffer: Upload heap type buffer connect between GPU and CPU.
* m_mappedData: the data mapped to the upload buffer.

{% highlight cpp %}
template<typename T>
class UploadBuffer
{
public:
	UploadBuffer(ID3D12Device* device, UINT elementNum, bool isConstantBuffer);
	~UploadBuffer();
	UploadBuffer(const UploadBuffer& buffer) = delete;
	UploadBuffer& operator=(const UploadBuffer& buffer) = delete;

	//Function
	ID3D12Resource* GetResource() const;
	void CopyData(int elementIndex, const T& data);

private:
	Microsoft::WRL::ComPtr<ID3D12Resource> m_uploadBuffer;
	BYTE* m_mappedData = nullptr;
	UINT m_elementSize = 0;
	bool mIsConstantBuffer = false;
};
{% endhighlight %}

The relation ship between the mappedData and uploadBuffer is:

{% highlight cpp %}
m_elementSize = sizeof(T);
//Create Updata Load Buffer
ThrowIfFailed(device->CreateCommittedResource(
		&CD3DX12_HEAP_PROPERTIES(D3D12_HEAP_TYPE_UPLOAD),
		D3D12_HEAP_FLAG_NONE,
		&CD3DX12_RESOURCE_DESC::Buffer(m_elementSize * elementNum),
		D3D12_RESOURCE_STATE_GENERIC_READ,
		nullptr, IID_PPV_ARGS(&m_uploadBuffer)));
//Upload Buffer 
ThrowIfFailed(m_uploadBuffer->Map(0, nullptr, reinterpret_cast<void**>(&m_mappedData)));
{% endhighlight %}



### Frame Resource

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

The Frame Resource data structure framework.

{% highlight cpp %}
//FrameBuffer
class FrameResource
{
public:
	FrameResource(ID3D12Device* device, UINT passCount, UINT objectCount, UINT materialCount, UINT waveVertCount);
	FrameResource(const FrameResource& rs) = delete;
	FrameResource& operator=(const FrameResource& rs) = delete;
	~FrameResource() {};

	Microsoft::WRL::ComPtr<ID3D12CommandAllocator> m_commandListAllocator;
	std::unique_ptr<UploadBuffer<RenderPassConstants>> m_passCB = nullptr;
	std::unique_ptr<UploadBuffer<SGDX12::MaterialConstants>> m_materialCB = nullptr;
	std::unique_ptr<UploadBuffer<ObjectConstants>> m_objectCB = nullptr;

	UINT64 m_fence = 0;
};
{% endhighlight %}

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


### Render Item



