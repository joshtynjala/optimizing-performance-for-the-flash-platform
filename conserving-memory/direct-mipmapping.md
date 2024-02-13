# Direct mipmapping

![](../img/tip_help.png) Use mipmapping to scale large images, if needed.
Another new feature available in Flash Player 10.1 and AIR 2.5 on all platforms
is related to mipmapping. Flash Player 9 and AIR 1.0 introduced a mipmapping
feature that improved the quality and performance of downscaled bitmaps. Note:
The mipmapping feature applies only to dynamically loaded images or embedded
bitmaps. Mipmapping does not apply to display objects that have been filtered or
cached. Mipmapping can be processed only if the bitmap has a width and height
that are even numbers. When a width or height that is an odd number is
encountered, mipmapping stops. For example, a 250 x 250 image can be mipmapped
down to 125 x 125, but it cannot be mipmapped further. In this case, at least
one of the dimensions is an odd number. Bitmaps with dimensions that are powers
of two achieve the best results, for example: 256 x 256, 512 x 512, 1024 x 1024,
and so on. As an example, imagine that a 1024 x 1024 image is loaded, and a
developer wants to scale the image to create a thumbnail in a gallery. The
mipmapping feature renders the image properly when scaled by using the
intermediate downsampled versions of the bitmap as textures. Previous versions
of the runtime created intermediate downscaled versions of the bitmap in memory.
If a 1024 x 1024 image was loaded and displayed at 64 x 64, older versions of
the runtime would create every half-sized bitmap. For example, in this case 512
x 512, 256 x 256, 128 x 128, and 64 x 64 bitmaps would be created.

Flash Player 10.1 and AIR 2.5 now support mipmapping directly from the original
source to the destination size required. In the previous example, only the 4 MB
(1024 x 1024) original bitmap and the 16 KB (64 x 64) mipmapped bitmap would be
created.

The mipmapping logic also works with the dynamic bitmap unloading feature. If
only the 64 x 64 bitmap is used, the 4-MB original bitmap is freed from memory.
If the mipmap must be recreated, then the original is reloaded. Also, if other
mipmapped bitmaps of various sizes are required, the mipmap chain of bitmaps is
used to create the bitmap. For example, if a 1:8 bitmap must be created, the 1:4
and 1:2 and 1:1 bitmaps are examined to determine which is loaded into memory
first. If no other versions are found, the 1:1 original bitmap is loaded from
the resource and used.

The JPEG decompressor can perform mipmapping within its own format. This direct
mipmapping allows a large bitmap to be decompressed directly to a mipmap format
without loading the entire uncompressed image. Generating the mipmap is
substantially faster, and memory used by large bitmaps is not allocated and then
freed. The JPEG image quality is comparable to the general mipmapping technique.
Note: Use mipmapping sparingly. Although it improves the quality of downscaled
bitmaps, it has an impact on bandwidth, memory, and speed. In some cases, a
better option can be to use a pre-scaled version of the bitmap from an external
tool and import it into your application. Don’t start with large bitmaps if you
only intend to scale them down.
