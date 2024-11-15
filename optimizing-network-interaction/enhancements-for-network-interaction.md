# Enhancements for network interaction

Flash Player 10.1 and AIR 2.5 introduce a set of new features for network
optimization on all platforms, including circular buffering and smart seeking.

## Circular buffering

When loading media content on mobile devices, you can encounter problems that
you would almost never expect on a desktop computer. For example, you are more
likely to run out of disk space or memory. When loading video, the desktop
versions of Flash Player 10.1 and AIR 2.5 download and cache the entire FLV file
(or MP4 file) onto the hard drive. The runtime then plays back the video from
that cache file. It is unusual for the disk space to run out. If such a
situation occurs, the desktop runtime stops the playback of the video.

A mobile device can run out of disk space more easily. If the device runs out of
disk space, the runtime does not stop the playback, as on the desktop runtime.
Instead, the runtime starts to reuse the cache file by writing to it again from
the beginning of the file. The user can continue watching the video. The user
cannot seek in the area of the video that has been rewritten to, except to the
beginning of the file. Circular buffering is not started by default. It can be
started during playback, and also at the beginning of playback if the movie is
bigger than the disk space or RAM. The runtime requires at least 4 MB of RAM or
20 MB of disk space to be able to use circular buffering.

> **Note:** If the device has enough disk space, the mobile version of the
> runtime behaves the same as on the desktop. Keep in mind that a buffer in RAM
> is used as a fallback if the device does not have a disk or the disk is full.
> A limit for the size of the cache file and the RAM buffer can be set at
> compile time. Some MP4 files have a structure that requires the whole file is
> downloaded before the playback can start. The runtime detects those files and
> prevents the download, if there is not enough disk space, and the MP4 file
> cannot be played back. It can be best not to request the download of those
> files at all.

As a developer, remember that seeking only works within the boundary of the
cached stream. `NetStream.seek()` sometimes fails if the offset is out of range,
and in this case, a `NetStream.Seek.InvalidTime` event is dispatched.

## Smart seeking

> **Note:** The smart seeking feature requires Adobe® Flash® Media Server 3.5.3.

Flash Player 10.1 and AIR 2.5 introduce a new behavior, called smart seeking,
which improves the user's experience when playing streaming video. If the user
seeks a destination inside the buffer bounds, the runtime reuses the buffer to
offer instant seeking. In previous versions of the runtime, the buffer was not
reused. For example, if a user was playing a video from a streaming server and
the buffer time was set to 20 seconds ( `NetStream.bufferTime`), and the user
tried to seek 10 seconds ahead, the runtime would throw away all the buffer data
instead of reusing the 10 seconds already loaded. This behavior forced the
runtime to request new data from the server much more frequently and cause poor
playback performance on slow connections.

The figure below illustrates how the buffer behaved in the previous releases of
the runtime. The `bufferTime` property specifies the number of seconds to
preload ahead so that if connection drops the buffer can be used without
stopping the video:

![](../img/on_buffer.png)

Buffer behavior before the smart seeking feature

With the smart seeking feature, the runtime now uses the buffer to provide
instant backward or forward seeking when the user scrubs the video. The
following figure illustrates the new behavior:

![](../img/on_buffer_delay.png)

Forward seeking with the smart seeking feature

![](../img/on_buffer_smartseeking.png)

Backward seeking with the smart seeking feature

Smart seeking reuses the buffer when the user seeks forward or backward, so that
playback experience is faster and smoother. One of the benefits of this new
behavior is bandwidth savings for video publishers. However, if the seeking is
outside the buffer limits, standard behavior occurs, and the runtime requests
new data from the server.

> **Note:** This behavior does not apply to progressive video download.

To use smart seeking, set `NetStream.inBufferSeek` to `true`.
