---
layout: post
title:  "Memory Allocator"
date:   2023-12-15
excerpt: "The memory allocator pool for mini engine."
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


## 1. Linear Allocator Page Manager

The Allocator has two types, **LinearAllocatorType**:
* kGpuExclusive = 0: DEFAULT GPU-writeable (via UAV)
* kCpuWritable = 1: UPLOAD CPU-writeable (but write combined)

The Linear Allocator has two page sizes:
* kGpuAllocatorPageSize = 0x10000: a linear page which has 64K for gpu allocation.
* kCpuAllocatorPageSize = 0x200000: a linear page which has 2MB for cpu allocation.

The basic members in a linear allocator is shown as follows:

```
LinearAllocatorType m_AllocationType;

std::vector<std::unique_ptr<LinearAllocationPage>> m_PagePool;

std::queue<std::pair<uint64_t, LinearAllocationPage*>> m_RetiredPages;

std::queue<std::pair<uint64_t, LinearAllocationPage*>> m_DeletionQueue;

std::queue<LinearAllocationPage*> m_AvailablePages;

std::mutex m_Mutex;
```

### 1.1 Request Page

The request pages function **returns a LinearAllocationPage* pointer**. Considering multi-threading situation, we should lock the mutex to make sure the code is a critical section. 

```
lock_guard<mutex> LockGuard(m_Mutex);
```

First, we recycle the content in retired pages. We check whether the fence value of retired value is finished. If complete, we push the linear allocation page to the availiable vector.

```
while (!m_RetiredPages.empty() && g_CommandManager.IsFenceComplete(m_RetiredPages.front().first))
{
    m_AvailablePages.push(m_RetiredPages.front().second);
    m_RetiredPages.pop();
}    
```

Secondly, we check the availiable pages vector. If there is available page, we return the target pages. Otherwise, we create a new page.

```
LinearAllocationPage* PagePtr = nullptr;

if (!m_AvailablePages.empty())
{
    PagePtr = m_AvailablePages.front();
    m_AvailablePages.pop();
}
else
{
    PagePtr = CreateNewPage();
    m_PagePool.emplace_back(PagePtr);
}
return PagePtr;
```

### 1.2 Create New Page

Create a default D3D12_HEAP_PROPERTIES, we just **fill the structure with default value**. The d3d12 heap is **the basis of the resource**.

```
    D3D12_HEAP_PROPERTIES HeapProps;
    HeapProps.CPUPageProperty = D3D12_CPU_PAGE_PROPERTY_UNKNOWN;
    HeapProps.MemoryPoolPreference = D3D12_MEMORY_POOL_UNKNOWN;
    HeapProps.CreationNodeMask = 1;
    HeapProps.VisibleNodeMask = 1;
```

We fill the resource descriptor D3D12_RESOURCE_DESC with default values as follows.

```
    D3D12_RESOURCE_DESC ResourceDesc;
    ResourceDesc.Dimension = D3D12_RESOURCE_DIMENSION_BUFFER;
    ResourceDesc.Alignment = 0;
    ResourceDesc.Height = 1;
    ResourceDesc.DepthOrArraySize = 1;
    ResourceDesc.MipLevels = 1;
    ResourceDesc.Format = DXGI_FORMAT_UNKNOWN;
    ResourceDesc.SampleDesc.Count = 1;
    ResourceDesc.SampleDesc.Quality = 0;
    ResourceDesc.Layout = D3D12_TEXTURE_LAYOUT_ROW_MAJOR;
```

Considering the type of the memory allocator, we allocate different page size for different type allocation page. The resource flag is also different:
* the gpu type allocator: D3D12_RESOURCE_FLAG_ALLOW_UNORDERED_ACCESS
* the cpu type allocator: D3D12_RESOURCE_FLAG_NONE

In DirectX 12, when creating a resource, we typically use the D3D12_RESOURCE_DESC structure to describe **the properties of the resource**. The flags related to resource creation are usually specified in the Flags member of the D3D12_RESOURCE_DESC structure.

```
    D3D12_RESOURCE_STATES DefaultUsage;
    if (m_AllocationType == kGpuExclusive)
    {
        HeapProps.Type = D3D12_HEAP_TYPE_DEFAULT;
        ResourceDesc.Width = PageSize == 0 ? kGpuAllocatorPageSize : PageSize;
        ResourceDesc.Flags = D3D12_RESOURCE_FLAG_ALLOW_UNORDERED_ACCESS;
        DefaultUsage = D3D12_RESOURCE_STATE_UNORDERED_ACCESS;
    }
    else
    {
        HeapProps.Type = D3D12_HEAP_TYPE_UPLOAD;
        ResourceDesc.Width = PageSize == 0 ? kCpuAllocatorPageSize : PageSize;
        ResourceDesc.Flags = D3D12_RESOURCE_FLAG_NONE;
        DefaultUsage = D3D12_RESOURCE_STATE_GENERIC_READ;
    }
```

Finally, we create the ID3D12Resource instance by the device->CreateCommittedResource function. It should be noticed that, we use the HeapProps and ResourceDesc discussed above.

```
ID3D12Resource* pBuffer;
ASSERT_SUCCEEDED( g_Device->CreateCommittedResource(&HeapProps, D3D12_HEAP_FLAG_NONE,
    &ResourceDesc, DefaultUsage, nullptr, MY_IID_PPV_ARGS(&pBuffer)));

pBuffer->SetName(L"LinearAllocator Page");

return new LinearAllocationPage(pBuffer, DefaultUsage);
```
