---
Description: Setting the Group Media Type
ms.assetid: '05f0fdcb-74a4-441e-ac3c-d3d2c1dfee80'
title: Setting the Group Media Type
---

# Setting the Group Media Type

\[This API is not supported and may be altered or unavailable in the future.\]

All groups must define an uncompressed media type, either audio or video. The uncompressed media type is the format that viewers see or hear during playback. Typically, the final output will be in a compressed format. For more information, see [Rendering a Project](rendering-a-project.md).

To set the uncompressed format, create an [**AM\_MEDIA\_TYPE**](am-media-type.md) structure and fill it with the appropriate major type, subtype, and format header. For video, allocate a [**VIDEOINFOHEADER**](videoinfoheader.md) structure for the format block, and set the width, height, and bit depth. For audio, allocate a [**WAVEFORMATEX**](waveformatex.md) structure for the format block, and set the sample rate, bit depth, and number of channels. If you set just the major type, DES provides reasonable defaults for the other values. In practice, you should set the values explicitly to control the output.

After you initialize the media type structure, call the [**IAMTimelineGroup::SetMediaType**](iamtimelinegroup-setmediatype.md) method to set the media type for the group.

The following example specifies 16-bit RGB video, 320 pixels wide by 240 pixels high:


```C++
AM_MEDIA_TYPE mtGroup;  
mtGroup.majortype = MEDIATYPE_Video;
mtGroup.subtype = MEDIASUBTYPE_RGB555;

// Set format headers.
mtGroup.pbFormat = (BYTE*)CoTaskMemAlloc(sizeof(VIDEOINFOHEADER));
if (mtGroup.pbFormat == NULL)
{
    return E_OUTOFMEMORY;
}

VIDEOINFOHEADER *pVideoHeader = (VIDEOINFOHEADER*)mtGroup.pbFormat;
ZeroMemory(pVideoHeader, sizeof(VIDEOINFOHEADER));
pVideoHeader->bmiHeader.biBitCount = 16;
pVideoHeader->bmiHeader.biWidth = 320;
pVideoHeader->bmiHeader.biHeight = 240;
pVideoHeader->bmiHeader.biPlanes = 1;
pVideoHeader->bmiHeader.biSize = sizeof(BITMAPINFOHEADER);
pVideoHeader->bmiHeader.biSizeImage = DIBSIZE(pVideoHeader->bmiHeader);

// Set the format type and size.
mtGroup.formattype = FORMAT_VideoInfo;
mtGroup.cbFormat = sizeof(VIDEOINFOHEADER);

// Set the sample size.
mtGroup.bFixedSizeSamples = TRUE;
mtGroup.lSampleSize = DIBSIZE(pVideoHeader->bmiHeader);

// Now use this media type for the group.
pGroup->SetMediaType(&amp;mtGroup);

// Clean up.
CoTaskMemFree(mtGroup.pbFormat);
```



The next example specifies an audio group, by setting the group media type to 16-bit stereo, 44100 samples per second:


```C++
AM_MEDIA_TYPE mt;  
ZeroMemory(&amp;mt, sizeof(AM_MEDIA_TYPE));

mt.majortype = MEDIATYPE_Audio;
mt.subtype = MEDIASUBTYPE_PCM;
mt.formattype = FORMAT_WaveFormatEx;

// Set format block.
mt.pbFormat = (BYTE*)CoTaskMemAlloc(sizeof(WAVEFORMATEX));
if (mt.pbFormat == NULL)
{
    return E_OUTOFMEMORY;
}
mt.cbFormat = sizeof(WAVEFORMATEX);

// Fill in the WAVEFORMATEX structure.
WAVEFORMATEX *wave = (WAVEFORMATEX*) mt.pbFormat;
wave->wFormatTag = WAVE_FORMAT_PCM;
wave->nChannels = 2;  // Stereo
wave->nSamplesPerSec = 44100;
wave->wBitsPerSample = 16;
wave->nBlockAlign = wave->nChannels * wave->wBitsPerSample/8;
wave->nAvgBytesPerSec = wave->nSamplesPerSec * wave->nBlockAlign; 
wave->cbSize = 0;

hr = pGroup->SetMediaType(&amp;mt);
CoTaskMemFree(mt.pbFormat);
```



You can also use the [**CMediaType**](cmediatype.md) class in the [DirectShow Base Classes](directshow-base-classes.md) to manage media types. It contains some useful helper methods, and automatically releases the format block.

## Related topics

<dl> <dt>

[About Media Types](about-media-types.md)
</dt> <dt>

[Constructing a Timeline](constructing-a-timeline.md)
</dt> </dl>

 

 


