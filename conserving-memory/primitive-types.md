# Primitive types

![](../img/tip_help.png) Use the `getSize()` method to benchmark code and
determine the most efficient object for the task. All primitive types except
String use 4 – 8 bytes in memory. There is no way to optimize memory by using a
specific type for a primitive:

    // Primitive types
    var a:Number;
    trace(getSize(a));
    // output: 8

    var b:int;
    trace(getSize(b));
    // output: 4

    var c:uint;
    trace(getSize(c));
    // output: 4

    var d:Boolean;
    trace(getSize(d));
    // output: 4

    var e:String;
    trace(getSize(e));
    // output: 4

A Number, which represents a 64-bit value, is allocated 8 bytes by the
ActionScript Virtual Machine (AVM), if it is not assigned a value. All other
primitive types are stored in 4 bytes.

    // Primitive types
    var a:Number = 8;
    trace(getSize(a));
    // output: 4

    a = Number.MAX_VALUE;
    trace(getSize(a));
    // output: 8

The behavior differs for the String type. The amount of storage allocated is
based on the length of the String:

    var name:String;
    trace(getSize(name));
    // output: 4

    name = "";
    trace(getSize(name));
    // output: 24

    name = "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularized in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.";
    trace(getSize(name));
    // output: 1172

Use the `getSize()` method to benchmark code and determine the most efficient
object for the task.
