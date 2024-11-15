# Freeing memory

> ![](../img/tip_help.png) Delete all references to objects to make sure that
> garbage collection is triggered.

You cannot launch the garbage collector directly in the release version of Flash
Player. To make sure that an object is garbage collected, delete all references
to the object. Keep in mind that the old `delete` operator used in ActionScript
1.0 and 2.0 behaves differently in ActionScript 3.0. It can only be used to
delete dynamic properties on a dynamic object.

> **Note:** You can call the garbage collector directly in Adobe® AIR® and in
> the debug version of Flash Player.

For example, the following code sets a Sprite reference to `null`:

    var mySprite:Sprite = new Sprite();

    // Set the reference to null, so that the garbage collector removes
    // it from memory
    mySprite = null;

Remember that when an object is set to `null`, it is not necessarily removed
from memory. Sometimes the garbage collector does not run, if available memory
is not considered low enough. Garbage collection is not predictable. Memory
allocation, rather than object deletion, triggers garbage collection. When the
garbage collector runs, it finds graphs of objects that haven't been collected
yet. It detects inactive objects in the graphs by finding objects that reference
each other, but that the application no longer uses. Inactive objects detected
this way are deleted.

In large applications, this process can be CPU-intensive and can affect
performance and generate a noticeable slowdown in the application. Try to limit
garbage collection passes by reusing objects as much as possible. Also, set
references to null, when possible, so that the garbage collector spends less
processing time finding the objects. Think of garbage collection as insurance,
and always manage object lifetimes explicitly, when possible.

> **Note:** Setting a reference to a display object to null does not ensure that
> the object is frozen. The object continues consume CPU cycles until it is
> garbage collected. Make sure that you properly deactivate your object before
> setting its reference to `null`.

The garbage collector can be launched using the `System.gc()` method, available
in Adobe AIR and in the debug version of Flash Player. The profiler bundled with
Adobe® Flash® Builder™ allows you to start the garbage collector manually.
Running the garbage collector allows you to see how your application responds
and whether objects are correctly deleted from memory.

> **Note:** If an object was used as an event listener, another object can
> reference it. If so, remove event listeners using the `removeEventListener()`
> method before setting the references to `null`.

Fortunately, the amount of memory used by bitmaps can be instantly reduced. For
example, the BitmapData class includes a `dispose()` method. The next example
creates a BitmapData instance of 1.8 MB. The current memory in use grows to 1.8
MB, and the `System.totalMemory` property returns a smaller value:

    trace(System.totalMemory / 1024);
    // output: 43100

    // Create a BitmapData instance
    var image:BitmapData = new BitmapData(800, 600);

    trace(System.totalMemory / 1024);
    // output: 44964

Next, the BitmapData is manually removed (disposed) from memory and the memory
use is once again checked:

    trace(System.totalMemory / 1024);
    // output: 43100

    // Create a BitmapData instance
    var image:BitmapData = new BitmapData(800, 600);

    trace(System.totalMemory / 1024);
    // output: 44964

    image.dispose();
    image = null;

    trace(System.totalMemory / 1024);
    // output: 43084

Although the `dispose()` method removes the pixels from memory, the reference
must still be set to `null` to release it completely. Always call the
`dispose()` method and set the reference to `null` when you no longer need a
BitmapData object, so that memory is freed immediately.

> **Note:** Flash Player 10.1 and AIR 1.5.2 introduce a new method called
> `disposeXML()` on the System class. This method allows you to make an XML
> object immediately available for garbage collection, by passing the XML tree
> as a parameter.

More Help topics

[Freezing and unfreezing objects](../minimizing-cpu-usage.md#freezing-and-unfreezing-objects)
