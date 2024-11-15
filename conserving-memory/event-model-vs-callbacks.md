# Event model versus callbacks

> ![](../img/tip_help.png) Consider using simple callbacks, instead of the event
> model.

The ActionScript 3.0 event model is based on the concept of object dispatching.
The event model is object-oriented and optimized for code reuse. The
`dispatchEvent()` method loops through the list of listeners and calls the event
handler method on each registered object. However, one of the drawbacks of the
event model is that you are likely to create many objects over the lifetime of
your application.

Imagine that you must dispatch an event from the timeline, indicating the end of
an animation sequence. To accomplish the notification, you can dispatch an event
from a specific frame in the timeline, as the following code illustrates:

    dispatchEvent( new Event ( Event.COMPLETE ) );

The Document class can listen for this event with the following line of code:

    addEventListener( Event.COMPLETE, onAnimationComplete );

Although this approach is correct, using the native event model can be slower
and consume more memory than using a traditional callback function. Event
objects must be created and allocated in memory, which creates a performance
slowdown. For example, when listening to the Event.ENTER_FRAME event, a new
event object is created on each frame for the event handler. Performance can be
especially slow for display objects, due to the capture and bubbling phases,
which can be expensive if the display list is complex.
