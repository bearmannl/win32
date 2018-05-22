﻿---
Description: 'Allows the callback object to specify the number of items in the folder view. Used by IShellFolderViewCB::MessageSFVCB.'
title: 'SFVM\_DEFITEMCOUNT message'
---

# SFVM\_DEFITEMCOUNT message

Allows the callback object to specify the number of items in the folder view. Used by [**IShellFolderViewCB::MessageSFVCB**](ishellfolderviewcb-messagesfvcb.md).


```C++
SFVM_DEFITEMCOUNT 

    lParam = (LPARAM)(UINT*) cItems;

            
```



## Parameters

<dl> <dt>

*cItems* \[out\]
</dt> <dd>

The number of items in the folder view.

</dd> </dl>

## Requirements



|                                     |                                                                                     |
|-------------------------------------|-------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows 2000 Professional \[desktop apps only\]<br/>                          |
| Minimum supported server<br/> | Windows 2000 Server \[desktop apps only\]<br/>                                |
| Header<br/>                   | <dl> <dt>Shlobj.h</dt> </dl> |



 

 



