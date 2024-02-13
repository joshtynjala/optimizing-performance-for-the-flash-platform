# Tips for optimizing GPU rendering performance

Although GPU rendering can greatly improve performance of SWF content, the
content's design plays an important role. Remember that settings that have
historically worked well in software rendering sometimes do not work well with
GPU rendering. The following tips can help you achieve good performance with GPU
rendering without incurring any performance penalty in software rendering. Note:
In order to leverage GPU acceleration of Flash content with AIR for mobile
platforms, Adobe recommends that you use renderMode="direct" (that is, Stage3D)
rather than renderMode="gpu". Adobe officially supports and recommends the
following Stage3D based frameworks: Starling (2D) and Away3D (3D). For more
details on Stage3D and Starling/Away3D, see
<http://gaming.adobe.com/getstarted/>.

- Avoid using `wmode=transparent` or `wmode=opaque` in HTML embed parameters.
  These modes can result in decreased performance. They can also result in a
  small loss in audio-video synchronization in both software and hardware
  rendering. Furthermore, many platforms do not support GPU rendering when these
  modes are in effect, significantly impairing performance.

- Use only the normal and alpha blend modes. Avoid using other blend modes,
  especially the layer blend mode. Not all blend modes can be reproduced
  faithfully when rendered with the GPU.

- When a GPU renders vector graphics, it breaks them up into meshes made of
  small triangles before drawing them. This process is called tessellating.
  Tessellating incurs a small performance cost, which increases as the
  complexity of the shape increases. To minimize performance impact, avoid morph
  shapes, which GPU rendering tessellates on every frame.

- Avoid self-intersecting curves, very thin curved regions (such as a thin
  crescent moon), and intricate details along the edges of a shape. These shapes
  are complex for the GPU to tessellate into triangle meshes. To understand why,
  consider two vectors: a 500 × 500 square and a 100 × 10 crescent moon. A GPU
  can easily render the large square because it's just two triangles. However,
  it takes many triangles to describe the curve of the crescent moon. Therefore,
  rendering the shape is more complicated even though it involves fewer pixels.

- Avoid large changes in scale, because such changes can also cause the GPU to
  again tessellate the graphics.

- Avoid overdrawing whenever possible. Overdrawing is layering multiple
  graphical elements so that they obscure each other. Using the software
  renderer, each pixel is drawn only once. Therefore, for software rendering,
  the application incurs no performance penalty regardless how many graphical
  elements are covering each other at that pixel location. By contrast, the
  hardware renderer draws each pixel for each element whether other elements
  obscure that region or not. If two rectangles overlap each other, the hardware
  renderer draws the overlapped region twice while the software renderer draws
  the region only once.

  Therefore, on the desktop, which use the software renderer, you typically do
  not notice a performance impact of overdraw. However, many overlapping shapes
  can adversely affect performance on devices using GPU rendering. A best
  practice is to remove objects from the display list rather than hiding them.

- Avoid using a large filled rectangle as a background. Set the background color
  of the Stage instead.

- Avoid the default bitmap fill mode of bitmap repeat whenever possible. Use
  bitmap clamp mode instead to achieve better performance.
