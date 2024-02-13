# Audio

Starting with Flash Player 9.0.115.0 and AIR 1.0, the runtime can play AAC files
(AAC Main, AAC LC, and SBR). A simple optimization can be made by using AAC
files instead of mp3 files. The AAC format offers better quality and smaller
file size than the mp3 format at an equivalent bitrate. Reducing file size saves
bandwidth, which is an important factor on mobile devices that don’t offer
high-speed Internet connections.

## Hardware Audio Decoding

Similar to video decoding, audio decoding requires high CPU cycles and can be
optimized by leveraging available hardware on the device. Flash Player 10.1 and
AIR 2.5 can detect and use hardware audio drivers to improve performance when
decoding AAC files (LC, HE/SBR profiles) or mp3 files (PCM is not supported).
CPU usage is reduced dramatically, which results in less battery usage and makes
the CPU available for other operations. Note: When using the AAC format, the AAC
Main profile is not supported on devices due to the lack of hardware support on
most devices. Hardware audio decoding is transparent to the user and developer.
When the runtime starts playing audio streams, it checks the hardware first, as
it does with video. If a hardware driver is available and the audio format is
supported, hardware audio decoding takes place. However, even if the incoming
AAC or mp3 stream decoding can be handled through the hardware, sometimes the
hardware cannot process all effects. For example, sometimes the hardware does
not process audio mixing and resampling, depending on hardware limitations.
