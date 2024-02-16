# Alpha blending

![](../img/tip_help.png) Avoid using the alpha property, when possible. Avoid
using effects that require alpha blending when using the `alpha` property, such
as fading effects. When a display object uses alpha blending, the runtime must
combine the color values of every stacked display object and the background
color to determine the final color. Thus, alpha blending can be more
processor-intensive than drawing an opaque color. This extra computation can
hurt performance on slow devices. Avoid using the `alpha` property, when
possible.

More Help topics

[Bitmap caching](./bitmap-caching/index.md)

[Rendering text objects](./rendering-text-objects.md)
