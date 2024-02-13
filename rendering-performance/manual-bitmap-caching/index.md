# Manual bitmap caching

![](../../img/tip_help.png) Use the BitmapData class to create custom bitmap
caching behavior. The following example reuses a single rasterized bitmap
version of a display object and references the same BitmapData object. When
scaling each display object, the original BitmapData object in memory is not
updated and is not redrawn. This approach saves CPU resources and makes
applications run faster. When scaling the display object, the contained bitmap
is stretched.

Here is the updated `BitmapApple` class:

    package org.bytearray.bitmap
    {
        import flash.display.Bitmap;
        import flash.display.BitmapData;
        import flash.events.Event;

        public class BitmapApple extends Bitmap
        {
            private var destinationX:Number;
            private var destinationY:Number;

            public function BitmapApple(buffer:BitmapData)
            {
                super(buffer);

                addEventListener(Event.ADDED_TO_STAGE,activation);
                addEventListener(Event.REMOVED_FROM_STAGE,deactivation);
            }

            private function activation(e:Event):void
            {
                initPos();
                addEventListener(Event.ENTER_FRAME,handleMovement);
            }

            private function deactivation(e:Event):void
            {
                removeEventListener(Event.ENTER_FRAME,handleMovement);
            }

            private function initPos():void
            {
                destinationX = Math.random()*(stage.stageWidth - (width>>1));
                destinationY = Math.random()*(stage.stageHeight - (height>>1));
            }

            private function handleMovement(e:Event):void
            {
                alpha = Math.random();

                x -= (x - destinationX)*.5;
                y -= (y - destinationY)*.5;

                if ( Math.abs(x - destinationX) < 1 && Math.abs(y - destinationY) < 1)
                    initPos();
            }
        }
    }

The alpha value is still modified on each frame. The following code passes the
original source buffer to each BitmapApple instance:

    import org.bytearray.bitmap.BitmapApple;

    const MAX_NUM:int = 100;
    var holder:Sprite = new Sprite();

    addChild(holder);

    var holderVector:Vector.<BitmapApple> = new Vector.<BitmapApple>(MAX_NUM, true);
    var source:AppleSource = new AppleSource();
    var bounds:Object = source.getBounds(source);

    var mat:Matrix = new Matrix();
    mat.translate(-bounds.x,-bounds.y);

    var buffer:BitmapData = new BitmapData(source.width+1, source.height+1, true, 0);
    buffer.draw(source,mat);

    var bitmapApple:BitmapApple;

    for (var i:int = 0; i< MAX_NUM; i++)
    {
        bitmapApple = new BitmapApple(buffer);

        holderVector[i] = bitmapApple;

        holder.addChild(bitmapApple);
    }

This technique uses only a small amount of memory, because only a single cached
bitmap is used in memory and shared by all the `BitmapApple` instances. In
addition, despite any modifications that are made to the `BitmapApple`
instances, such as alpha, rotation, or scaling, the original source bitmap is
never updated. Using this technique prevents a performance slowdown.

For a smooth final bitmap, set the `smoothing` property to `true`:

    public function BitmapApple(buffer:BitmapData)
    {
        super (buffer);

        smoothing = true;

        addEventListener(Event.ADDED_TO_STAGE, activation);
        addEventListener(Event.REMOVED_FROM_STAGE, deactivation);
    }

Adjusting the Stage quality can also improve performance. Set the Stage quality
to `HIGH` before rasterization, then switch to `LOW` afterwards:

    import org.bytearray.bitmap.BitmapApple;

    const MAX_NUM:int = 100;
    var holder:Sprite = new Sprite();

    addChild ( holder );

    var holderVector:Vector.<BitmapApple> = new Vector.<BitmapApple>(MAX_NUM, true);
    var source:AppleSource = new AppleSource();
    var bounds:Object = source.getBounds ( source );

    var mat:Matrix = new Matrix();
    mat.translate ( -bounds.x, -bounds.y );

    var buffer:BitmapData = new BitmapData ( source.width+1, source.height+1, true, 0 );

    stage.quality = StageQuality.HIGH;

    buffer.draw ( source, mat );
     
    stage.quality = StageQuality.LOW;

    var bitmapApple:BitmapApple;

    for (var i:int = 0; i< MAX_NUM; i++ )
    {
        bitmapApple = new BitmapApple( buffer );

        holderVector[i] = bitmapApple;

        holder.addChild ( bitmapApple );
    }

Toggling Stage quality before and after drawing the vector to a bitmap can be a
powerful technique to get anti-aliased content onscreen. This technique can be
effective regardless of the final stage quality. For example, you can get an
anti-aliased bitmap with anti-aliased text, even with the Stage quality set to
`LOW`. This technique is not possible with the `cacheAsBitmap` property. In that
case, setting the Stage quality to `LOW` updates the vector quality, which
updates the bitmap surfaces in memory, and updates the final quality.

- [Isolating behaviors](WS4bebcd66a74275c36c11f3d612431904db9-7ffa.html)
