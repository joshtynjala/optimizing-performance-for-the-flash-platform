# Movie quality

> ![](../img/tip_help.png) Use the appropriate Stage quality setting to improve
> rendering.

When developing content for mobile devices with small screens, such as phones,
image quality is less important than when developing desktop applications.
Setting the Stage quality to the appropriate setting can improve rendering
performance.

The following settings are available for Stage quality:

- `StageQuality.LOW`: Favors playback speed over appearance and does not use
  anti-aliasing. This setting is not supported in Adobe AIR for desktop or TV.

- `StageQuality.MEDIUM`: Applies some anti-aliasing but does not smooth scaled
  bitmaps. This setting is the default value for AIR on mobile devices, but is
  not supported in AIR for desktop or TV.

- `StageQuality.HIGH`: (Default on the desktop) Favors appearance over playback
  speed and always uses anti-aliasing. If the SWF file does not contain
  animation, scaled bitmaps are smoothed; if the SWF file contains animation,
  bitmaps are not smoothed.

- `StageQuality.BEST`: Provides the best display quality and does not consider
  playback speed. All output is anti-aliased and scaled bitmaps are always
  smoothed.

Using `StageQuality.MEDIUM` often provides enough quality for applications on
mobile devices, and in some cases using `StageQuality.LOW` can provide enough
quality. Since Flash Player 8, anti-aliased text can be rendered accurately,
even with Stage quality set to `LOW`.

> **Note:** On some mobile devices, even though the quality is set to `HIGH`,
> `MEDIUM` is used for better performance in Flash Player applications. However,
> setting the quality to HIGH often does not make a noticeable difference,
> because mobile screens usually have a higher dpi. (The dpi can vary depending
> on the device.)

In the following figure, the movie quality is set to `MEDIUM` and the text
rendering is set to Anti-Alias for Animation:

![](../img/or_medquality_antialias.png)

Medium Stage quality and text rendering set to Anti-Alias for Animation

The Stage quality setting affects the text quality, because the appropriate text
rendering setting is not being used.

The runtime allows you to set text rendering to Anti-Alias for Readability. This
setting maintains perfect quality of your (anti-aliased) text regardless of
which Stage quality setting you use:

![](../img/or_lowquality_antialias.png)

Low Stage quality and text rendering set to Anti-Alias for Readability

The same rendering quality can be obtained by setting text rendering to Bitmap
Text (No anti-alias):

![](../img/or_lowquality_bitmaptext_noantialias.png)

Low Stage quality and text rendering set to Bitmap Text (No anti-alias)

The last two examples show that you can get high-quality text, no matter which
Stage quality setting you use. This feature has been available since Flash
Player 8 and can be used on mobile devices. Keep in mind that Flash Player 10.1
automatically switches to `StageQuality.MEDIUM` on some devices to increase
performance.
