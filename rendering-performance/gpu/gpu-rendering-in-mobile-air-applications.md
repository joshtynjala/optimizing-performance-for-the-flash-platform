# GPU rendering in mobile AIR applications

Enable hardware graphics acceleration in an AIR application by including
`<renderMode>gpu</renderMode>` in the application descriptor (or use
`<renderMode>direct</renderMode>` to leverage Stage3D). You cannot change render
modes at runtime. On desktop computers, using `<renderMode>gpu</renderMode>`
will fallback to `<renderMode>direct</renderMode>`. Note: In order to leverage
GPU acceleration of Flash content with AIR for mobile platforms, Adobe
recommends that you use renderMode="direct" (that is, Stage3D) rather than
renderMode="gpu". Adobe officially supports and recommends the following Stage3D
based frameworks: Starling (2D) and Away3D (3D). For more details on Stage3D and
Starling/Away3D, see
<https://web.archive.org/web/20140209182418/http://gaming.adobe.com/getstarted/>.

#### GPU rendering mode limitations

The following limitations exist when using GPU rendering mode in AIR 2.5 and
above:

- If the GPU cannot render an object, it is not displayed at all. There is no
  fallback to CPU rendering.

- The following blend modes are not supported: layer, alpha, erase, overlay,
  hardlight, lighten, and darken.

- Filters are not supported.

- PixelBender is not supported.

- Many GPU units have a maximum texture size of 1024x1024. In ActionScript, this
  translates to the maximum final rendered size of a display object after all
  transformations.

- Adobe does not recommend the use of GPU rendering mode in AIR applications
  that play video.

- In GPU rendering mode, text fields are not always moved to a visible location
  when the virtual keyboard opens. To ensure that your text field is visible
  while the user enters text, do one of the following. Place the text field in
  the top half of the screen or move it to the top half of the screen when it
  receives focus.

- GPU rendering mode is disabled for some devices on which the mode does not
  work reliably. See the AIR developer release notes for the latest information.

#### GPU rendering mode best practices

The following guidelines can make GPU rendering faster:

- Limit the numbers of items visible on stage. Each item takes some time to
  render and composite with the other items around it. When you no longer want
  to display a display object, set its `visible` property to `false`. Do not
  simply move it off stage, hide it behind another object, or set its `alpha`
  property to 0. If the display object is no longer needed at all, remove it
  from the stage with `removeChild()`.

- Reuse objects rather than creating and destroying them.

- Make bitmaps in sizes that are close to, but less than, 2 <sup>n</sup> by 2
  <sup>m</sup> bits. The dimensions do not have to be exact powers of 2, but
  they should be close to a power of 2, without being larger. For example, a
  31-by-15-pixel image renders faster than a 33-by-17-pixel image. (31 and 15
  are just less than powers of 2: 32 and 16.)

- If possible, set the repeat parameter to false when calling the
  `Graphic.beginBitmapFill()` method.

- Don't overdraw. Use the background color as a background. Don't layer large
  shapes on top of each other. There is a cost for every pixel that must be
  drawn.

- Avoid shapes with long thin spikes, self -intersecting edges, or lots of fine
  detail along the edges. These shapes take longer to render than display
  objects with smooth edges.

- Limit the size of display objects.

- Enable cacheAsBitMap and cacheAsBitmapMatrix for display objects whose
  graphics aren't updated frequently.

- Avoid using the ActionScript drawing API (the Graphics class) to create
  graphics. When possible, create those objects statically at authoring time
  instead.

- Scale bitmap assets to the final size before importing them.

#### GPU rendering mode in mobile AIR 2.0.3 and above

GPU rendering is more restrictive in mobile AIR apps created with the Packager
for iPhone. The GPU is only effective for bitmaps, solid shapes, and display
objects that have the `cacheAsBitmap` property set. Also, for objects that have
`cacheAsBitmap` and `cacheAsBitmapMatrix` set, the GPU can effectively render
objects that rotate or scale. The GPU is used in tandem for other display
objects and this generally results in poor rendering performance.
