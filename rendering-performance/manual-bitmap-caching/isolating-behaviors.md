# Isolating behaviors

![](../../img/tip_help.png) Isolate events such as the `Event.ENTER_FRAME` event
in a single handler, when possible. The code can be further optimized by
isolating the `Event.ENTER_FRAME` event in the `Apple` class in a single
handler. This technique saves CPU resources. The following example shows this
different approach, where the BitmapApple class no longer handles the movement
behavior:

    package org.bytearray.bitmap
    {
    	import flash.display.Bitmap;
    	import flash.display.BitmapData;

    	public class BitmapApple extends Bitmap
    	{
    		private var destinationX:Number;
    		private var destinationY:Number;

    		public function BitmapApple(buffer:BitmapData)
    		{
    			super (buffer);

    			smoothing = true;
    		}
    	}
    }

The following code instantiates the apples and handles their movement in a
single handler:

    import org.bytearray.bitmap.BitmapApple;

    const MAX_NUM:int = 100;
    var holder:Sprite = new Sprite();

    addChild(holder);

    var holderVector:Vector.<BitmapApple> = new Vector.<BitmapApple>(MAX_NUM, true);
    var source:AppleSource = new AppleSource();
    var bounds:Object = source.getBounds(source);

    var mat:Matrix = new Matrix();
    mat.translate(-bounds.x,-bounds.y);

    stage.quality = StageQuality.BEST;

    var buffer:BitmapData = new BitmapData(source.width+1,source.height+1, true,0);
    buffer.draw(source,mat);

    stage.quality = StageQuality.LOW;

    var bitmapApple:BitmapApple;

    for (var i:int = 0; i< MAX_NUM; i++)
    {
    	bitmapApple = new BitmapApple(buffer);

    	bitmapApple.destinationX = Math.random()*stage.stageWidth;
    	bitmapApple.destinationY = Math.random()*stage.stageHeight;

    	holderVector[i] = bitmapApple;

    	holder.addChild(bitmapApple);
    }

    stage.addEventListener(Event.ENTER_FRAME,onFrame);

    var lng:int = holderVector.length

    function onFrame(e:Event):void
    {
        for (var i:int = 0; i < lng; i++)
        {
            bitmapApple = holderVector[i];
            bitmapApple.alpha = Math.random();

            bitmapApple.x -= (bitmapApple.x - bitmapApple.destinationX) *.5;
            bitmapApple.y -= (bitmapApple.y - bitmapApple.destinationY) *.5;

            if (Math.abs(bitmapApple.x - bitmapApple.destinationX ) < 1 &&
                Math.abs(bitmapApple.y - bitmapApple.destinationY ) < 1)
            {
                    bitmapApple.destinationX = Math.random()*stage.stageWidth;
                    bitmapApple.destinationY = Math.random()*stage.stageHeight;
            }
        }
    }

The result is a single `Event.ENTER_FRAME` event handling the movement, instead
of 200 handlers moving each apple. The whole animation can be paused easily,
which can be useful in a game.

For example, a simple game can use the following handler:

    stage.addEventListener(Event.ENTER_FRAME, updateGame);
    function updateGame (e:Event):void
    {
    	gameEngine.update();
    }

The next step is to make the apples interact with the mouse or keyboard, which
requires modifications to the `BitmapApple` class.

    package org.bytearray.bitmap
    {
    	import flash.display.Bitmap;
    	import flash.display.BitmapData;
    	import flash.display.Sprite;

    	public class BitmapApple extends Sprite
    	{
    		public var destinationX:Number;
    		public var destinationY:Number;
    		private var container:Sprite;
    		private var containerBitmap:Bitmap;

    		public function BitmapApple(buffer:BitmapData)
    		{
    			container = new Sprite();
    			containerBitmap = new Bitmap(buffer);
    			containerBitmap.smoothing = true;
    			container.addChild(containerBitmap);
    			addChild(container);
    		}
    	}
    }

The result is `BitmapApple` instances that are interactive, like traditional
Sprite objects. However, the instances are linked to a single bitmap, which is
not resampled when the display objects are transformed.
