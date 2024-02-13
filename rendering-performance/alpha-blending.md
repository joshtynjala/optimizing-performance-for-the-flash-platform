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

[Bitmap caching](WS4bebcd66a74275c36c11f3d612431904db9-7ffc.html)

[Rendering text objects](WS4bebcd66a74275c36c11f3d612431904db9-7ff9.html)
