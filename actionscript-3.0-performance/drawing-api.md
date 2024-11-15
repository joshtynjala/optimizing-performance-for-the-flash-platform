# Drawing API

> ![](../img/tip_help.png) Use the drawing API for faster code execution.

Flash Player 10 and AIR 1.5 provided a new drawing API, which allows you to get
better code execution performance. This new API does not provide rendering
performance improvement, but it can dramatically reduce the number of lines of
code you have to write. Fewer lines of code can provide better ActionScript
execution performance.

The new drawing API includes the following methods:

- `drawPath()`

- `drawGraphicsData()`

- `drawTriangles()`

> **Note:** This discussion does not focus on the `drawTriangles()` method,
> which is related to 3D. However, this method can improve ActionScript
> performance, because it handles native texture mapping.

The following code explicitly calls the appropriate method for each line being
drawn:

      var container:Shape = new Shape();
      container.graphics.beginFill(0x442299);

      var coords:Vector.<Number> = Vector.<Number>([132, 20, 46, 254, 244, 100, 20, 98, 218, 254]);

      container.graphics.moveTo ( coords[0], coords[1] );
      container.graphics.lineTo ( coords[2], coords[3] );
      container.graphics.lineTo ( coords[4], coords[5] );
      container.graphics.lineTo ( coords[6], coords[7] );
      container.graphics.lineTo ( coords[8], coords[9] );

      addChild( container );

The following code runs faster than the previous example, because it executes
fewer lines of code. The more complex the path, the greater the performance gain
from using the `drawPath()` method:

    var container:Shape = new Shape();
    container.graphics.beginFill(0x442299);

    var commands:Vector.<int> = Vector.<int>([1,2,2,2,2]);
    var coords:Vector.<Number> = Vector.<Number>([132, 20, 46, 254, 244, 100, 20, 98, 218, 254]);

    container.graphics.drawPath(commands, coords);

    addChild( container );

The `drawGraphicsData()` method provides similar performance improvements.
