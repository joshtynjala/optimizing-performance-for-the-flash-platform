# Working with pixels

![](../img/tip_help.png) Paint pixels using the `setVector()` method. When
painting pixels, some simple optimizations can be made just by using the
appropriate methods of the BitmapData class. A fast way to paint pixels is to
use the `setVector()` method:

    // Image dimensions
    var wdth:int = 200;
    var hght:int = 200;
    var total:int = wdth*hght;
     
    // Pixel colors Vector
    var pixels:Vector.<uint> = new Vector.<uint>(total, true);
     
    for ( var i:int = 0; i< total; i++ )
    {
        // Store the color of each pixel
        pixels[i] = Math.random()*0xFFFFFF;
    }
     
    // Create a non-transparent BitmapData object
    var myImage:BitmapData = new BitmapData ( wdth, hght, false );
    var imageContainer:Bitmap = new Bitmap ( myImage );
     
    // Paint the pixels
    myImage.setVector ( myImage.rect, pixels );
    addChild ( imageContainer );

When using slow methods, such as `setPixel()` or `setPixel32()`, use the
`lock()` and `unlock()` methods to make things run faster. In the following
code, the `lock(`) and `unlock()` methods are used to improve performance:

    var buffer:BitmapData = new BitmapData(200,200,true,0xFFFFFFFF);
    var bitmapContainer:Bitmap = new Bitmap(buffer);
    var positionX:int;
    var positionY:int;
     
    // Lock update
    buffer.lock();
    var starting:Number=getTimer();
     
    for (var i:int = 0; i<2000000; i++)
    {
        // Random positions
        positionX = Math.random()*200;
        positionY = Math.random()*200;
        // 40% transparent pixels
        buffer.setPixel32( positionX, positionY, 0x66990000 );
    }
     
    // Unlock update
    buffer.unlock();
    addChild( bitmapContainer );
     
    trace( getTimer () - starting );
    // output : 670

The `lock()` method of the BitmapData class locks an image and prevents objects
that reference it from being updated when the BitmapData object changes. For
example, if a Bitmap object references a BitmapData object, you can lock the
BitmapData object, change it, and then unlock it. The Bitmap object is not
changed until the BitmapData object is unlocked. To improve performance, use
this method along with the `unlock()` method before and after numerous calls to
the `setPixel()` or `setPixel32()` method. Calling `lock()` and `unlock()`
prevents the screen from being updated unnecessarily. Note: When processing
pixels on a bitmap not on the display list (double-buffering), sometimes this
technique does not improve performance. If a bitmap object does not reference
the bitmap buffer, using `lock()` and `unlock()` does not improve performance.
Flash Player detects that the buffer is not referenced, and the bitmap is not
rendered onscreen. Methods that iterate over pixels, such as `getPixel()`,
`getPixel32()`, `setPixel()`, and `setPixel32()`, are likely to be slow,
especially on mobile devices. If possible, use methods that retrieve all the
pixels in one call. For reading pixels, use the `getVector()` method, which is
faster than the `getPixels()` method. Also, remember to use APIs that rely on
Vector objects, when possible, as they are likely to run faster.
