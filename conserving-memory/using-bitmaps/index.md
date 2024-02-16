# Using bitmaps

Using vectors instead of bitmaps is good way to save memory. However, using
vectors, especially in large numbers, dramatically increases the need for CPU or
GPU resources. Using bitmaps is a good way to optimize rendering, because the
runtime needs fewer processing resources to draw pixels on the screen than to
render vector content.

- [Bitmap downsampling](./bitmap-downsampling.md)
- [BitmapData single reference](./bitmapdata-single-reference.md)

More Help topics

[Manual bitmap caching](../../rendering-performance/manual-bitmap-caching/index.md)
