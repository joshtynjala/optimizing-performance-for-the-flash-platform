# GPU rendering in Flash Player applications

An important new feature of Flash Player 10.1 is that it can use the GPU to
render graphical content on mobile devices. In the past, graphics were rendered
through the CPU only. Using the GPU optimizes the rendering of filters, bitmaps,
video, and text. Keep in mind that GPU rendering is not always as accurate as
software rendering. Content can look slightly chunky when using the hardware
renderer. In addition, Flash Player 10.1 has a limitation that can prevent
onscreen Pixel Bender effects from rendering. These effects can render as a
black square when using hardware acceleration.

Although Flash Player 10 had a GPU acceleration feature, the GPU was not used to
calculate the graphics. It was only used to send all the graphics to the screen.
In Flash Player 10.1, the GPU is also used to calculate the graphics, which can
improve rendering speed significantly. It also reduces the CPU workload, which
is helpful on devices with limited resources, such as mobile devices.

GPU mode is set automatically when running content on mobile devices, for the
best possible performance. Although `wmode` no longer must be set to `gpu` to
get GPU rendering, setting `wmode` to `opaque` or `transparent` disables GPU
acceleration.

> **Note:** Flash Player on the desktop still uses the CPU to do software
> rendering. Software rendering is used because drivers vary widely on the
> desktop, and drivers can accentuate rendering differences. There can also be
> rendering differences between the desktop and some mobile devices.
