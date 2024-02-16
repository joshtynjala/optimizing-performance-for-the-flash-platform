# Object pooling

![](../../img/tip_help.png) Use object pooling, when possible.

Another important optimization is called object pooling, which involves reusing
objects over time. You create a defined number of objects during the
initialization of your application and store them inside a pool, such as an
Array or Vector object. Once you are done with an object, you deactivate it so
that it does not consume CPU resources, and you remove all mutual references.
However, you do not set the references to `null`, which would make it eligible
for garbage collection. You just put the object back into the pool, and retrieve
it when you need a new object.

Reusing objects reduces the need to instantiate objects, which can be expensive.
It also reduces the chances of the garbage collector running, which can slow
down your application. The following code illustrates the object pooling
technique:

    package
    {
    	import flash.display.Sprite;

    	public final class SpritePool
    	{
    		private static var MAX_VALUE:uint;
    		private static var GROWTH_VALUE:uint;
    		private static var counter:uint;
    		private static var pool:Vector.<Sprite>;
    		private static var currentSprite:Sprite;
    	 
    		public static function initialize( maxPoolSize:uint, growthValue:uint ):void
    		{
    			MAX_VALUE = maxPoolSize;
    			GROWTH_VALUE = growthValue;
    			counter = maxPoolSize;

    			var i:uint = maxPoolSize;

    			pool = new Vector.<Sprite>(MAX_VALUE);
    			while( --i > -1 )
    				pool[i] = new Sprite();
    		}

    		public static function getSprite():Sprite
    		{
    			if ( counter > 0 )
    				return currentSprite = pool[--counter];

    			var i:uint = GROWTH_VALUE;
    			while( --i > -1 )
    					pool.unshift ( new Sprite() );
    			counter = GROWTH_VALUE;
    			return getSprite();

    		}
    	 
    		public static function disposeSprite(disposedSprite:Sprite):void
    		{
    			pool[counter++] = disposedSprite;
    		}
    	}
    }

The SpritePool class creates a pool of new objects at the initialization of the
application. The `getSprite()` method returns instances of these objects, and
the `disposeSprite()` method releases them. The code allows the pool to grow
when it has been consumed completely. It’s also possible to create a fixed-size
pool where new objects would not be allocated when the pool is exhausted. Try to
avoid creating new objects in loops, if possible. For more information, see
[Freeing memory](../freeing-memory.md). The following code uses the SpritePool
class to retrieve new instances:

    const MAX_SPRITES:uint = 100;
    const GROWTH_VALUE:uint = MAX_SPRITES >> 1;
    const MAX_NUM:uint = 10;
     
    SpritePool.initialize ( MAX_SPRITES,  GROWTH_VALUE );
     
    var currentSprite:Sprite;
    var container:Sprite = SpritePool.getSprite();
     
    addChild ( container );
     
    for ( var i:int = 0; i< MAX_NUM; i++ )
    {
    	for ( var j:int = 0; j< MAX_NUM; j++ )
    	{
    		currentSprite = SpritePool.getSprite();
    		currentSprite.graphics.beginFill ( 0x990000 );
    		currentSprite.graphics.drawCircle ( 10, 10, 10 );
    		currentSprite.x = j * (currentSprite.width + 5);
    		currentSprite.y = i * (currentSprite.width + 5);
    		container.addChild ( currentSprite );
    	}
    }

The following code removes all the display objects from the display list when
the mouse is clicked, and reuses them later for another task:

    stage.addEventListener ( MouseEvent.CLICK, removeDots );
     
    function removeDots ( e:MouseEvent ):void
    {
    	while (container.numChildren > 0 )
    		SpritePool.disposeSprite (container.removeChildAt(0) as Sprite );
    }

Note: The pool vector always references the Sprite objects. If you want to
remove the object from memory completely, you would need a `dispose()` method on
the SpritePool class, which would remove all remaining references.
