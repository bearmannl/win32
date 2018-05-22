---
Description: 'The ASF file sink is an implementation of IMFMediaSink provided by Media Foundation that an application can use to archive ASF media data to a file. For information about ASF Media Sinks' object model and general usage, see ASF Media Sinks.'
ms.assetid: 'ecfddf4e-71b4-42c4-8b54-9868cec6ed9b'
title: Adding Metadata to the File Sink
---

# Adding Metadata to the File Sink

The ASF file sink is an implementation of [**IMFMediaSink**](imfmediasink.md) provided by Media Foundation that an application can use to archive ASF media data to a file. For information about ASF Media Sinks' object model and general usage, see [ASF Media Sinks](asf-media-sinks.md).

After [Creating the ASF file sink](creating-the-asf-file-sink.md), it must be configured with information about the streams and encoding information in the output file. These procedures are described in [Adding Stream Information to the ASF File Sink](adding-stream-information-to-the-asf-file-sink.md) and [Setting Properties in the File Sink](setting-properties-in-the-file-sink.md). In addition, You can also add metadata information includes name/value pairs such as "Author", Title". This topic describes the process of adding metadata information to the file sink so that it appears in the final [ASF Header Object](asf-file-structure.md#header-object).

You can add metadata information to the ASF file sink before building the encoding topology. The ASF ContentInfo object for the file sink keeps track of the metadata properties and are exposed to the application through the [**IMFMetadata**](imfmetadata.md) interface. Some of these properties, such as "IsVBR" that indicates if the file contains variable bit rate (VBR) streams, are set automatically by the file sink by parsing the stream encoding properties that are set.

For a complete list of properties, see the "Attribute List" topic in the Format SDK documentation.

## Using the IMFMetadata Interface on the ASF File Sink

1.  Query the ASF file sink object to get a pointer to its implementation of the [**IMFMetadataProvider**](imfmetadataprovider.md) interface.
2.  Call [**IMFMetadataProvider::GetMFMetadata**](imfmetadataprovider-getmfmetadata.md) to get an [**IMFMetadata**](imfmetadata.md) pointer.

    The *pPresentationDescriptor* parameter is ignored and the application can pass **NULL**. If the application passes zero as the stream identifier in the *dwStreamIdentifier* parameter, the method retrieves metadata that applies to the entire ASF file. Otherwise, only the metadata for the stream is retrieved.

3.  Call [**IMFMetadata::GetAllPropertyNames**](imfmetadata-getallpropertynames.md) to retrieve the list of file-encoding properties set on the media content.
4.  Call [**IMFMetadata::GetProperty**](imfmetadata-getproperty.md) to get the property values.

## Example

The following example code shows how to enumerate the property names and values that are set on the ASF file.


```C++
/////////////////////////////////////////////////////////////////////
// Name: ListASFProperties
//
// Enumerates the metadata properties of the ASF file. 
//
// pContentInfo: Pointer to the ASF ContentInfo object.
/////////////////////////////////////////////////////////////////////

HRESULT ListASFProperties(IMFASFContentInfo *pContentInfo)
{
    HRESULT hr = S_OK;
    
    PROPVARIANT varNames;
    PropVariantInit(&amp;varNames);

    PROPVARIANT varValue;
    PropVariantInit(&amp;varValue);

    IMFMetadataProvider* pProvider = NULL;
    IMFMetadata* pMetadata = NULL;

    // Query the ContentInfo object for IMFMetadataProvider.
    CHECK_HR(hr = pContentInfo->QueryInterface(IID_IMFMetadataProvider,
        (void**)&amp;pProvider));

    // Get a pointer to IMFMetadata for file-wide metadata.
    CHECK_HR(hr = pProvider->GetMFMetadata(NULL, 0, 0, &amp;pMetadata));

    // Get the property names that are stored in the metadata object.
    CHECK_HR(hr = pMetadata->GetAllPropertyNames(&amp;varNames));

    // Loop through the properties and get their values.
    if (varNames.vt == (VT_VECTOR | VT_LPWSTR))
    {
        ULONG cElements = varNames.calpwstr.cElems;
        for (ULONG i = 0; i < cElements; i++)
        {
            const WCHAR* sName = varNames.calpwstr.pElems[i];
            CHECK_HR(hr = pMetadata->GetProperty(sName, &amp;varValue));
            //Use the property values. Not shown.
            PropVariantClear(&amp;varValue);
        }
    }
done:
    PropVariantClear(&amp;varNames);
    PropVariantClear(&amp;varValue);
    SAFE_RELEASE (pMetaData);
    SAFE_RELEASE (pProvider);
    return hr;
}
```



## Related topics

<dl> <dt>

[ASF Media Sinks](asf-media-sinks.md)
</dt> <dt>

[Pipeline Layer ASF Components](pipeline-layer-asf-components.md)
</dt> <dt>

[ASF Support in Media Foundation](asf-support-in-media-foundation.md)
</dt> </dl>

 

 


