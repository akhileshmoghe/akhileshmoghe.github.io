---
title:  "Pravega + GStreamer Video Streaming Solution"
permalink: /_post/streaming_data/pravega_video_streaming
date:   2021-12-14 1:33:22 +0530
categories:
  - Pravega
  - Streming Data
  - Video Streaming
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Pravega
  - Streming Data
  - Video Streaming
author: Akhilesh Moghe
show_author_profile: true
---

## Pravega

### GStreamer
- __*<u>Pipeline</u>*__ based *<u>multimedia framework</u>* that links together a wide varity of media processing elements.
  - A Simple Pipeline example:

{% highlight shellscript %}
$ gst-launch v4l2src ! x264enc ! filesink location=myvideo.h264

- It's a pipeline with 3 functionalities/modules.
- v4l2src = captures the raw camera frames.
- x264enc = receives the raw frames from v4l2src and encodes each frame into h.264/mpeg-4 codec format.
- filesink = the encoded frames are dumped to a file on the dics.
{% endhighlight %}

- GStreamer also supports library to work with __*Python, C, C++, Rust, Java*__ language applications.
- *<u>Video players</u>*, *<u>Media Servers</u>*, *<u>Digital Video Recorder(DVR)</u>*, *<u>Video Management Servers(VMS)</u>* can be built with GStreamer.

#### Usages
- Play a media file from Web server
  ```
    $ gst-launch-1.0 playbin uri=https://upload.wikimedia.org/wikipedia/commons/d/d0/Foucault_pendulum_1.webm
  ```
- Continuously capture from network camera and write to a fragmented MP4 file
  ```
    $ gst-launch-1.0 rtspsrc location=rtsp://10.1.2.3/cam ! rtphh264depay ! h264parse ! mp4mux fragment-duration=100 ! filesink location=myvideo.mp4
    
    - fragments of 100ms
  ```
- Transcode a media file
  ```
    $ gst-launch-1.0 uridecodebin uri=https://upload.wikimedia.org/wikipedia/commons/d/d0/Foucault_pendulum_1.webm name=d ! queue ! theoraenc ! oggmux name=m ! filesink location=sintel.ogg.d ! queue ! audioconvert ! audioresample ! flacenc ! m
  ```
  - Pipelines can contain complex flows with splits and merges

## GStreamer Plugin for Pravega
### Pravega Sink
- Writes Video/Audio to Pravega Byte stream.
- Writes a *<u>time index</u>* to an auxiliary Pravega stream to enable efficient seeking.
  - Used to create a playlist for *<u>HTTP Live Straming (HLS)</u>*.
  - Writes an *index record* at every *Key Frame*.
- Used to write *<u>H.264 video & audio</u>* encoded as an MP4 (*<u>fragmented MP4 container format</u>*) or MPEG transport stream.
- Every write stores an *<u>Epoch timestamp</u>* as 64-bit nanoseconds from epoch time.

### Pravega Source
- Reads the byte stream written by Pravega Sink.
- *<u>Low end-to-end Latency</u>* tail reads (approx. 20ms).
- Seamless transition from *<u>Historical</u>* to *<u>live playback</u>*.
  - No difference in both of these, Pravega streams are just read with an Offset.
- Efficient seeking and range queries using time index
- Handles truncated stream (deleted data)
- Sink and Source written in *<u>Rust language</u>* with Pravega Rust Client, but Applications can be written in any language that GStreamer supports like Python, Java, C, Application just have to use the compiled Rust Library.
- Open-Source - Apache License

### Pravega Byte Stream Format for GStreamer
  ![pravega-gstreamer-data-index-byte-stream-format](/assets/images/streaming_data/pravega/pravega-gstreamer-data-index-byte-stream-format.png){:.shadow}
  
- Pravega streams are just Byte Streams, a sequence of bytes, no framing enforced, it can accept any kind of data. It's the application's responsibility to make sense of data.
- Index stream is only used for seeking and building an HTTP Live Streaming playlist
- Only Key frames are indexed.
- Every index stream packet is of 20 Bytes size.
- Both index and data streams are ordered.
- Application needs to make sure the truncation is in sync for both index and data streams. Truncation occurs only at atomic write boundries.
- Flags:
  - *<u>Discontinuity</u>*: Used by decoders to restart the decoding process after decoding a set of frames.
  - *<u>Random access</u>*: Used to indicate the *<u>Key frame</u>*
- Payload:
  - MPEG Transport stream packet: 188 Bytes {or} multiple of it {or}
  - MP4 fragment with video/audio frames {or}
  - a single Key frame {or} delta frame {or} mix of these two.
  - Max Size: 8 MB
- An Atomic write of the stream packets: i.e., either writes the complete or discards everything.


###### References:
- [Video in Pravega with GStreamer](https://www.youtube.com/watch?v=8MWexheVnHc&t=6s&ab_channel=PravegaIO)
- [pravega/gstreamer-pravega](https://github.com/pravega/gstreamer-pravega)
  - This project demonstrates methods to store, process, and read video with *<u>Pravega</u> and *<u>Flink</u>* which are key components of Dell EMC Streaming Data Platform (SDP).
  - It also includes an application that performs *<u>object detection</u>* using *<u>TensorFlow</u>* and *<u>YOLO</u>*.
  
  