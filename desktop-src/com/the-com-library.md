---
title: The COM Library
description: The COM Library
ms.assetid: '51d4db4a-ad88-4627-8140-2f7906945752'
---

# The COM Library

Any process that uses COM must both initialize and uninitialize the COM library. In addition to being a specification, COM also implements some important services in this library. Provided as a set of DLLs and EXEs (primarily Ole32.dll and Rpcss.exe) in Microsoft Windows, the COM library includes the following:

-   A small number of fundamental functions that facilitate the creation of COM applications, both client and server. For clients, COM supplies basic functions for creating objects. For servers, COM supplies the means of exposing their objects.

-   Implementation-locator services through which COM determines, from a unique class identifier (CLSID), which server implements that class and where that server is located. This service includes support for a level of indirection, usually a system registry, between the identity of an object class and the packaging of the implementation so that clients are independent of the packaging, which can change in the future.

-   Transparent remote procedure calls when an object is running in a local or remote server.

-   A standard mechanism to allow an application to control how memory is allocated within its process, particularly memory that needs to be passed between cooperating objects so that it can be freed properly.

To use basic COM services, all COM threads of execution in clients and out-of-process servers must call either the [**CoInitialize**](coinitialize.md) or the [**CoInitializeEx**](coinitializeex.md) function before calling any other COM function except memory allocation calls. **CoInitializeEx** replaces the other function, adding a parameter that allows you to specify the threading model of the threadâ€”either apartment-threaded or free-threaded. A call to **CoInitialize** simply sets the threading model to apartment-threaded.

OLE compound document applications call the [**OleInitialize**](oleinitialize.md) function, which calls [**CoInitializeEx**](coinitializeex.md) and also does some initialization required for compound documents. Therefore, threads that call **OleInitialize** cannot be free-threaded. For information on threading in clients and servers, see [Processes, Threads, and Apartments](processes--threads--and-apartments.md).

In-process servers do not call the initialization functions because they are being loaded into a process that has already done so. As a result, in-process servers must set their threading model in the registry under the [InprocServer32](inprocserver32.md) key. For detailed information on threading issues in in-process servers, see [In-Process Server Threading Issues](in-process-server-threading-issues.md).

It is also important to uninitialize the library. For each call to [**CoInitialize**](coinitialize.md) or [**CoInitializeEx**](coinitializeex.md), there must be a corresponding call to [**CoUninitialize**](couninitialize.md). For each call to [**OleInitialize**](oleinitialize.md), there must be a corresponding call to [**OleUninitialize**](oleuninitialize.md).

In-process servers can assume that the process they are being loaded into has already performed these steps.

## Related topics

<dl> <dt>

[The Component Object Model](the-component-object-model.md)
</dt> </dl>

 

 



