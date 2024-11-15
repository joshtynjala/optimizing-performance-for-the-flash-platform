# Event capture and bubbling

> ![](../img/tip_help.png) Use event capture and bubbling to minimize event
> handlers.

The event model in ActionScript 3.0 introduced the concepts of event capture and
event bubbling. Taking advantage of the bubbling of an event can help you to
optimize ActionScript code execution time. You can register an event handler on
one object, instead of multiple objects, to improve performance.

As an example, imagine creating a game in which the user must click apples as
fast as possible to destroy them. The game removes each apple from the screen
when it is clicked and adds points to the user's score. To listen to the
`MouseEvent.CLICK` event dispatched by each apple, you could be tempted to write
the following code:

    const MAX_NUM:int = 10;
    var sceneWidth:int = stage.stageWidth;
    var sceneHeight:int = stage.stageHeight;
    var currentApple:InteractiveObject;
    var currentAppleClicked:InteractiveObject;
     
    for ( var i:int = 0; i< MAX_NUM; i++ )
    {
        currentApple = new Apple();
        currentApple.x = Math.random()*sceneWidth;
        currentApple.y = Math.random()*sceneHeight;
        addChild ( currentApple );

        // Listen to the MouseEvent.CLICK event
        currentApple.addEventListener ( MouseEvent.CLICK, onAppleClick );
    }
     
    function onAppleClick ( e:MouseEvent ):void
    {
        currentAppleClicked = e.currentTarget as InteractiveObject;
        currentAppleClicked.removeEventListener(MouseEvent.CLICK, onAppleClick );
        removeChild ( currentAppleClicked );
    }

The code calls the `addEventListener()` method on each Apple instance. It also
removes each listener when an apple is clicked, using the
`removeEventListener()` method. However, the event model in ActionScript 3.0
provides a capture and bubbling phase for some events, allowing you to listen to
them from a parent InteractiveObject. As a result, it is possible to optimize
the code above and minimize the number of calls to the `addEventListener()` and
`removeEventListener()` methods. The following code uses the capture phase to
listen to the events from the parent object:

    const MAX_NUM:int = 10;
    var sceneWidth:int = stage.stageWidth;
    var sceneHeight:int = stage.stageHeight;
    var currentApple:InteractiveObject;
    var currentAppleClicked:InteractiveObject;
    var container:Sprite = new Sprite();
     
    addChild ( container );
     
    // Listen to the MouseEvent.CLICK on the apple's parent
    // Passing true as third parameter catches the event during its capture phase
    container.addEventListener ( MouseEvent.CLICK, onAppleClick, true );
     
    for ( var i:int = 0; i< MAX_NUM; i++ )
    {
        currentApple = new Apple();
        currentApple.x = Math.random()*sceneWidth;
        currentApple.y = Math.random()*sceneHeight;
        container.addChild ( currentApple );
    }
     
    function onAppleClick ( e:MouseEvent ):void
    {
        currentAppleClicked = e.target as InteractiveObject;
        container.removeChild ( currentAppleClicked );
    }

The code is simplified and much more optimized, with only one call to the
`addEventListener()` method on the parent container. Listeners are no longer
registered to the Apple instances, so there is no need to remove them when an
apple is clicked. The `onAppleClick()` handler can be optimized further, by
stopping the propagation of the event, which prevents it from going further:

    function onAppleClick ( e:MouseEvent ):void
    {
        e.stopPropagation();
        currentAppleClicked = e.target as InteractiveObject;
        container.removeChild ( currentAppleClicked );
    }

The bubbling phase can also be used to catch the event, by passing `false` as
the third parameter to the `addEventListener()` method:

    // Listen to the MouseEvent.CLICK on apple's parent
    // Passing false as third parameter catches the event during its bubbling phase
    container.addEventListener ( MouseEvent.CLICK, onAppleClick, false );

The default value for the capture phase parameter is `false`, so you can omit
it:

    container.addEventListener ( MouseEvent.CLICK, onAppleClick );
