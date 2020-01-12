---
layout: post
title: DVD Backup x265 
categories: [ffmpeg,mplayer,dvd,ddrescue]
---

Backup DVDs into x265 format.

Extract the VOB file using mplayer:

```bash
# MPlayer SVN-r38139
mplayer -dvd-device /dev/sr0 -nocache dvd://1 -dumpstream -dumpfile video.vob
```

If you get a read error during VOB extraction, use ddrescue to dump the disc to ISO first:

```bash
# GNU ddrescue 1.24
ddrescue -v -b 2048 -r 3 /dev/sr0 video.iso video.log
```

Then try the VOB extraction again using the iso:

```bash
mplayer -dvd-device video.iso -nocache dvd://1 -dumpstream -dumpfile video.vob
```

Analyze the file using ffmpeg to find the streams:

```bash
# ffmpeg version n4.2.1
ffmpeg -analyzeduration 500M -probesize 500M -i video.vob
```

[Convert closed captions to subrip if needed.](/2020/01/11/ffmpeg-convert-closed-captions/)

Do the final conversion. The following assumes you want to save the first three streams in the file:

```bash
ffmpeg -analyzeduration 500M -probesize 500M \
  -i video.vob \
  -map 0:0 -map 0:1 -map 0:2 \ # save the first three streams
  -c:v libx265 -crf 18 -b:v 0 \ # convert video with x265
  -c:a copy \ # copy audio as is
  -c:s copy \ # copy subtitles as is
  -metadata title="Movie Title Here" \
  outputfile.mkv
```
