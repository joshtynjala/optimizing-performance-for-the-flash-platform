# Cached bitmap transform matrixes in AIR

![](../../img/tip_help.png) Set the `cacheAsBitmapMatrix` property when using
cached bitmaps in mobile AIR apps. In the AIR mobile profile, you can assign a
Matrix object to the `cacheAsBitmapMatrix` property of a display object. When
you set this property, you can apply any two-dimensional transformation to the
object without regenerating the cached bitmap. You can also change the alpha
property without regenerating the cached bitmap. The `cacheAsBitmap` property
must also be set to `true` and the object must not have any 3D properties set.

Setting the `cacheAsBitmapMatrix` property generates the cached bitmap even if
the display object is off screen, hidden from view, or has its `visible`
property set to `false`. Resetting the `cacheAsBitmapMatrix` property using a
matrix object that contains a different transform also regenerates the cached
bitmap.

The matrix transformation you apply to the `cacheAsBitmapMatrix` property is
applied to the display object as it is rendered into the bitmap cache. Thus, if
the transform contains a 2x scale, the bitmap rendering is twice the size of the
vector rendering. The renderer applies the inverse transformation to the cached
bitmap so that the final display looks the same. You can scale the cached bitmap
to a smaller size to reduce memory usage, possibly at the expense of rendering
fidelity. You can also scale the bitmap to a larger size to increase rendering
quality in some cases, at the expense of increased memory usage. In general,
though, use an identity matrix, which is a matrix that applies no
transformation, to avoid changes in appearance, as shown in the following
example:

    displayObject.cacheAsBitMap = true;
    displayObject.cacheAsBitmapMatrix = new Matrix();

Once the `cacheAsBitmapMatrix` property is set, you can scale, skew, rotate, and
translate the object without triggering bitmap regeneration.

You can also change the alpha value within the range of 0 and 1. If you change
the alpha value through the `transform.colorTransform` property with a color
transform, the alpha used in the transform object must be within 0 and 255.
Changing the color transformation in any other way regenerates the cached
bitmap.

Always set the `cacheAsBitmapMatrix` property whenever you set `cacheAsBitmap`
to `true` in content created for mobile devices. However, consider the following
potential drawback. After an object is rotated, scaled or skewed, the final
rendering can exhibit bitmap scaling or aliasing artifacts compared to a normal
vector rendering.
