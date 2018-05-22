﻿---
Description: 'The Sources property enumerates all the sources for the product instance.'
ms.assetid: '26602099-d0e0-4269-91d9-82943859811a'
title: 'Product.Sources property'
---

# Product.Sources property

The **Sources** property enumerates all the sources for the product instance. This property calls [**MsiSourceListEnumSources**](msisourcelistenumsources.md) and returns the array of strings, and accepts the source type as argument. The source type can be MSISOURCETYPE\_NETWORK or MSISOURCETYPE\_URL.

This property is read-only.

## Syntax


```JScript
propVal = Product.Sources
```



## Property value

The type of source to enumerate. The value can be *msiInstallSourceTypeNetwork* (1) or *msiInstallSourceTypeURL* (2).

## Requirements



|                    |                                                                                                                                                                                                                                                                                      |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Version<br/> | Windows Installer 5.0 on Windows Server 2012, Windows 8, Windows Server 2008 R2 or Windows 7. Windows Installer 4.0 or Windows Installer 4.5 on Windows Server 2008 or Windows Vista. Windows Installer 3.0 or later on Windows Server 2003, Windows XP, and Windows 2000<br/> |
| DLL<br/>     | <dl> <dt>Msi.dll</dt> </dl>                                                                                                                                                                                                   |
| IID<br/>     | IID\_IProduct is defined as 000C10A0-0000-0000-C000-000000000046<br/>                                                                                                                                                                                                          |



## See also

<dl> <dt>

[**Product**](product-object.md)
</dt> <dt>

[**MsiSourceListEnumSources**](msisourcelistenumsources.md)
</dt> <dt>

[Not Supported in Windows Installer 2.0 and earlier](not-supported-in-windows-installer-version-2-0.md)
</dt> </dl>

 

 



