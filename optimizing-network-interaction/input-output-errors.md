# Input output errors

> ![](../img/tip_help.png) Provide event handlers and error messages for IO
> errors.

On a mobile device, the network can be less reliable as on a desktop computer
connected to high-speed Internet. Accessing external content on mobile devices
has two constraints: availability and speed. Therefore, make sure that assets
are lightweight and add handlers for every IO_ERROR event to provide feedback to
the user.

For example, imagine a user is browsing your website on a mobile device and
suddenly loses the network connection between two metro stations. A dynamic
asset was being loaded when the connection is lost. On the desktop, you can use
an empty event listener to prevent a runtime error from showing, because this
scenario would almost never happen. However, on a mobile device you must handle
the situation with more than just a simple empty listener.

The following code does not respond to an IO error. Do not use it as it is
shown:

    var loader:Loader = new Loader();
    loader.contentLoaderInfo.addEventListener( Event.COMPLETE, onComplete );
    addChild( loader );
    loader.load( new URLRequest ("asset.swf" ) );
     
    function onComplete( e:Event ):void
    {
    var loader:Loader = e.currentTarget.loader;
    loader.x = ( stage.stageWidth - e.currentTarget.width ) >> 1;
    loader.y = ( stage.stageHeight - e.currentTarget.height ) >> 1;
    }

A better practice is to handle such a failure and provide an error message for
the user. The following code handles it properly:

    var loader:Loader = new Loader();
    loader.contentLoaderInfo.addEventListener ( Event.COMPLETE, onComplete );
    loader.contentLoaderInfo.addEventListener ( IOErrorEvent.IO_ERROR, onIOError );
    addChild ( loader );
    loader.load ( new URLRequest ("asset.swf" ) );

    function onComplete ( e:Event ):void
    {
      var loader:Loader = e.currentTarget.loader;
      loader.x = ( stage.stageWidth - e.currentTarget.width ) >> 1;
      loader.y = ( stage.stageHeight - e.currentTarget.height ) >> 1;
    }

    function onIOError ( e:IOErrorEvent ):void
    {
      // Show a message explaining the situation and try to reload the asset.
      // If it fails again, ask the user to retry when the connection will be restored
    }

As a best practice, remember to offer a way for the user to load the content
again. This behavior can be implemented in the `onIOError()` handler.
