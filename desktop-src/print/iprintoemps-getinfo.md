﻿---
Description: 'A rendering plug-in''s IPrintOemPS::GetInfo method returns identification information.'
ms.assetid: '9a8b060d-675b-4171-b75e-6df55cd0667f'
title: 'IPrintOemPS::GetInfo method'
---

# IPrintOemPS::GetInfo method

A rendering plug-in's `IPrintOemPS::GetInfo` method returns identification information.

## Syntax


```C++
STDMETHOD GetInfo(
   DWORD  dwMode,
   PVOID  pBuffer,
   DWORD  cbSize,
   PDWORD pcbNeeded
);
```



## Parameters

<dl> <dt>

*dwMode* 
</dt> <dd>

Contains one of the following caller-supplied integer constants.

<dl> <dt>

<span id="OEMGI_GETPUBLISHERINFO"></span><span id="oemgi_getpublisherinfo"></span>OEMGI\_GETPUBLISHERINFO
</dt> <dd>

The method must indicate whether the rendering plug-in will be using "publishing mode". The *pBuffer* parameter points to a [**PUBLISHERINFO**](publisherinfo.md) structure, to be filled in by the method. For more information, see the following Remarks section.

</dd> </dl>

<dl> <dt>

<span id="OEMGI_GETREQUESTEDHELPERINTERFACES"></span><span id="oemgi_getrequestedhelperinterfaces"></span>OEMGI\_GETREQUESTEDHELPERINTERFACES
</dt> <dd>

The method must write the bit flag value of OEMPUBLISH\_IPRINTCOREHELPER to the buffer *pBuffer* if the [**IPrintOemPS::PublishDriverInterface**](iprintoemps-publishdriverinterface.md) method should be called with parameter *pIUnknown* pointing to an object that implements the [IPrintCoreHelperPS Interface](iprintcorehelperps-interface.md).

</dd> </dl>

<dl> <dt>

<span id="OEMGI_GETSIGNATURE"></span><span id="oemgi_getsignature"></span>OEMGI\_GETSIGNATURE
</dt> <dd>

The method must return a unique four-byte identification signature. The plug-in must also place this signature in [**OPTITEM**](optitem.md) structures, as described in the description of the [**OEMCUIPPARAM**](oemcuipparam.md). structure's **pOEMOptItems** member.

</dd> </dl>

<dl> <dt>

<span id="OEMGI_GETVERSION"></span><span id="oemgi_getversion"></span>OEMGI\_GETVERSION
</dt> <dd>

The method must return the user interface plug-in's version number as a DWORD. The version format is developer-defined.

</dd> </dl> </dd> <dt>

*pBuffer* 
</dt> <dd>

Caller-supplied pointer to memory allocated to receive the information specified by *dwMode*.

</dd> <dt>

*cbSize* 
</dt> <dd>

Caller-supplied size of the buffer pointed to by *pBuffer*.

</dd> <dt>

*pcbNeeded* 
</dt> <dd>

Caller-supplied pointer to a location to receive the number of bytes written into the buffer pointed to by *pBuffer*.

</dd> </dl>

## Return value

The method must return one of the following values.



| Return code                                                                               | Description                               |
|-------------------------------------------------------------------------------------------|-------------------------------------------|
| <dl> <dt>**S\_OK**</dt> </dl>      | The operation succeeded.<br/>       |
| <dl> <dt>**E\_FAIL**</dt> </dl>    | The operation failed<br/>           |
| <dl> <dt>**E\_NOTIMPL**</dt> </dl> | The method is not implemented.<br/> |



 

## Remarks

Rendering plug-ins for Pscript5 are required to implement the `IPrintOemPS::GetInfo` method, which is called immediately after the plug-in is loaded. The method should return the specified information by writing it to the address specified by *pBuffer* and writing the size, in bytes, of the returned information into the location specified by *pcbNeeded*.

If *pBuffer* is **NULL**, the method should just use *pcbNeeded* to return the number of bytes required to store the specified information.

In "publishing mode", all font information to be downloaded is placed in the job header. Each font is downloaded only once and can be used for any page, allowing the job to be page-order independent. If *dwMode* is OEMGI\_GETPUBLISHERINFO but the plug-in does not support publishing mode, `IPrintOemPS::GetInfo` should return E\_NOTIMPL.

For more information about creating and installing rendering plug-ins, see [Customizing Microsoft's Printer Drivers](print.customizing_microsoft_s_printer_drivers).

## Requirements



|                            |                                                                                                            |
|----------------------------|------------------------------------------------------------------------------------------------------------|
| Target platform<br/> | <dl> <dt>Desktop</dt> </dl>                         |
| Header<br/>          | <dl> <dt>Prcomoem.h (include Prcomoem.h)</dt> </dl> |



## See also

<dl> <dt>

[**IPrintOemPS**](iprintoemps-interface.md)
</dt> <dt>

[**IPrintOemUI::GetInfo**](iprintoemui-getinfo.md)
</dt> <dt>

[**IPrintOemUni::GetInfo**](iprintoemuni-getinfo.md)
</dt> </dl>

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bprint\print%5D:%20IPrintOemPS::GetInfo%20method%20%20RELEASE:%20%285/12/2018%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/en-us/default.aspx. "Send comments about this topic to Microsoft")




