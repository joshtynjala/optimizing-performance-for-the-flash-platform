# Display objects

![](../img/tip_help.png) Choose an appropriate display object. ActionScript 3.0
includes a large set of display objects. One of the most simple optimization
tips to limit memory usage is to use the appropriate type of display object. For
simple shapes that are not interactive, use Shape objects. For interactive
objects that don’t need a timeline, use Sprite objects. For animation that uses
a timeline, use MovieClip objects. Always choose the most efficient type of
object for your application.

The following code shows memory usage for different display objects:

    trace(getSize(new Shape()));
    // output: 236
     
    trace(getSize(new Sprite()));
    // output: 412
     
    trace(getSize(new MovieClip()));
    // output: 440

The `getSize()` method shows how many bytes an object consumes in memory. You
can see that using multiple MovieClip objects instead of simple Shape objects
can waste memory if the capabilities of a MovieClip object are not needed.
