# Bitmap downsampling

To make better use of memory, 32-bit opaque images are reduced to 16-bit images
when Flash Player detects a 16-bit screen. This downsampling consumes half the
memory resources, and images render more quickly. This feature is available only
in Flash Player 10.1 for Windows Mobile. Note: Before Flash Player 10.1, all
pixels created in memory were stored in 32 bits (4 bytes). A simple logo of 300
x 300 pixels consumed 350 KB of memory (300\*300\*4/1024). With this new
behavior, the same opaque logo consumes only 175 KB. If the logo is transparent,
it is not downsampled to 16 bits, and it keeps the same size in memory. This
feature applies only to embedded bitmaps or runtime-loaded images (PNG, GIF,
JPG). On mobile devices, it can be hard to tell the difference between an image
rendered in 16 bits and the same image rendered in 32 bits. For a simple image
containing just a few colors, there is no detectable difference. Even for a more
complex image, it is difficult to detect the differences. However, there can be
some color degradation when zooming in on the image, and a 16-bit gradient can
look less smooth than the 32-bit version.
