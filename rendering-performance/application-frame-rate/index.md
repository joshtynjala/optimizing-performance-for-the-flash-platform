# Application frame rate

![](../../img/tip_help.png) In general, use the lowest possible frame rate for
better performance. An application’s frame rate determines how much time is
available for each “application code and rendering” cycle, as described in
[Runtime code execution fundamentals](../../introduction/runtime-code-execution-fundamentals.md).
A higher frame rate creates smoother animation. However, when animation or other
visual changes aren’t happening, there is often no reason to have a high frame
rate. A higher frame rate expends more CPU cycles and energy from the battery
than a lower rate.

The following are some general guidelines for an appropriate default frame rate
for your application:

- If you are using the Flex framework, leave the starting frame rate at the
  default value.

- If your application includes animation, an adequate frame rate is at least 20
  frames per second. Anything more than 30 frames per second is often
  unnecessary.

- If your application doesn’t include animation, a frame rate of 12 frames per
  second is probably sufficient.

The “lowest possible frame rate” can vary depending on the current activity of
the application. See the next tip “Dynamically change the frame rate of your
application” for more information.

![](../../img/tip_help.png) Use a low frame rate when video is the only dynamic
content in your application. The runtime plays loaded video content at its
native frame rate, regardless of the frame rate of the application. If your
application has no animation or other rapidly changing visual content, using a
low frame rate does not degrade the experience of the user interface.

![](../../img/tip_help.png) Dynamically change the frame rate of your
application. You define the application’s initial frame rate in project or
compiler settings, but the frame rate is not fixed at that value. You can change
the frame rate by setting the `Stage.frameRate` property (or the
`WindowedApplication.frameRate` property in Flex).

Change the frame rate according to the current needs of your application. For
example, during a time when your application isn’t performing any animation,
lower the frame rate. When an animated transition is about to start, raise the
frame rate. Similarly, if your application is running in the background (after
having lost focus), you can usually lower the frame rate even further. The user
is likely to be focused on another application or task.

The following are some general guidelines to use as a starting point in
determining the appropriate frame rate for different types of activities:

- If you are using the Flex framework, leave the starting frame rate at the
  default value.

- When animation is playing, set the frame rate to at least 20 frames per
  second. Anything more than 30 frames per second is often unnecessary.

- When no animation is playing, a frame rate of 12 frames per second is probably
  sufficient.

- Loaded video plays at its native frame rate regardless of the application
  frame rate. If video is the only moving content in your application, a frame
  rate of 12 frames per second is probably sufficient.

- When the application doesn’t have input focus, a frame rate of 5 frames per
  second is probably sufficient.

- When an AIR application is not visible, a frame rate of 2 frames per second or
  less is probably appropriate. For example, this guideline applies if an
  application is minimized. It also applies on desktop devices if the native
  window’s `visible` property is `false`.

For applications built in Flex, the spark.components.WindowedApplication class
has built-in support for dynamically changing the application’s frame rate. The
`backgroundFrameRate` property defines the application’s frame rate when the
application isn’t active. The default value is 1, which changes the frame rate
of an application built with the Spark framework to 1 frame per second. You can
change the background frame rate by setting the `backgroundFrameRate` property.
You can set the property to another value, or set it to -1 to turn off automatic
frame rate throttling.

For more information on dynamically changing an application’s frame rate, see
the following articles:

- [Reducing CPU usage in Adobe AIR](https://web.archive.org/web/20120130203826/http://www.adobe.com/devnet/air/flex/articles/framerate_throttling.html)
  (Adobe Developer Center article and sample code by Jonnie Hallman)

- [Writing well-behaved, efficient, AIR applications](https://web.archive.org/web/20150228212345/http://arno.org/arnotify/2009/05/writing-well-behaved-efficient-air-applications/)
  (article and sample application by Arno Gourdol)

Grant Skinner has created a frame rate throttler class. You can use this class
in your applications to automatically decrease the frame rate when your
application is in the background. For more information and to download the
source code for the FramerateThrottler class, see Grant’s article Idle CPU Usage
in Adobe AIR and Flash Player at
<https://web.archive.org/web/20110811155738/http://gskinner.com/blog/archives/2009/05/idle_cpu_usage_.html>.

- [Adaptive frame rate](./adaptive-frame-rate.md)
