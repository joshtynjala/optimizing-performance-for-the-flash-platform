# Transparent windows

![](../img/tip_help.png) In AIR desktop applications, consider using an opaque
rectangular application window instead of a transparent window. To use an opaque
window for the initial window of an AIR desktop application, set the following
value in the application descriptor XML file:

    <initialWindow>
    	<transparent>false</transparent>
    </initialWindow>

For windows created by application code, create a NativeWindowInitOptions object
with the `transparent` property set to `false` (the default). Pass it to the
NativeWindow constructor while creating the NativeWindow object:

    // NativeWindow: flash.display.NativeWindow class

    var initOptions:NativeWindowInitOptions = new NativeWindowInitOptions();
    initOptions.transparent = false;
    var win:NativeWindow = new NativeWindow(initOptions);

For a Flex Window component, make sure the component’s transparent property is
set to false, the default, before calling the Window object’s `open()` method.

    // Flex window component: spark.components.Window class

    var win:Window = new Window();
    win.transparent = false;
    win.open();

A transparent window potentially shows part of the user’s desktop or other
application windows behind the application window. Consequently, the runtime
uses more resources to render a transparent window. A rectangular
non-transparent window, whether it uses operating system chrome or custom
chrome, does not have the same rendering burden.

Only use a transparent window when it is important to have a non-rectangular
display or to have background content display through your application window.
