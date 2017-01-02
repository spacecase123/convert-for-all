# convert-for-all

FFmpeg command-line wrapper.

# What does this thing do?

This program converts video files to be playable on these devices:

 - Google Chrome
 - Chromecast
 - Microsoft Xbox One

From the limited research I've performed, the most compatible container / codec
pairings to achieve this compatibility are:

 - Video codec: H264
 - Audio codec: AAC
 - File container: MP4

This program attempts to be intelligent(-ish) and take the shortest path to get
to the above results. That is, it will not re-encode streams which are already
in a compatible format. For example, if you have a `.avi` file with H264 video
and WAV audio, this program will inform FFmpeg to do the following things:

 - Pull apart the streams
 - Copy the video stream losslessly
 - Re-encode the audio stream to AAC
 - Stitch everything back together into an MP4 container

At the end of it all, you'll have a `<something-or-other>-compat.mp4` file
which should play on just about any device.

# System Requirements

 - [Ruby][ruby]
 - [FFmpeg][ffmpeg]

# Usage

```bash
$ convert-for-all <input_file(s)>
```

# Example

```bash
$ ls
my-video.avi

$ convert-for-all my-video.avi
Converting 'my-video.avi' with the following options:
{:audio_encode=>true, :video_encode=>false, :container_convert=>true}
Running this ffmpeg incantation:
ffmpeg -i my-video.avi -c:v copy -c:a aac ./my-video-compat.mp4
ffmpeg version ...
[snip]

$ ls
my-video.avi
my-video-compat.mp4
```

[ffmpeg]: https://ffmpeg.org/
[ruby]: https://www.ruby-lang.org/
