---
layout: post
title: ffmpeg Convert Closed Captions
categories: [ffmpeg]
---

Extract video closed captions and convert them to subrip format using ffmpeg.

```bash
# ffmpeg version n4.2.1
ffmpeg -f lavfi -i 'movie=video.file[out0+subcc]' -map s output.srt
```

Will extract the closed captions from the video file *video.file* into the output file *output.srt* in subrip format.

Closed captions are not the same as subtitle streams. Use this if the video stream info says *Closed Captions:*

```
Stream #0:0[0x100]: Video: mpeg2video (Main) ([2][0][0][0] / 0x0002),
yuv420p(tv, top first), 1920x1080 [SAR 1:1 DAR 16:9], Closed Captions,
29.97 fps, 29.97 tbr, 90k tbn, 59.94 tbc
```

You can then embed the converted closed captions as subtitles into another file:

```bash
ffmpeg -i input_video.mkv -i output.srt output_video.mkv
```
