# Text objects and memory

> ![](../img/tip_help.png) Use the Adobe® Flash® Text Engine for read-only text;
> use TextField objects for input text.

Flash Player 10 and AIR 1.5 introduced a powerful new text engine, the Adobe
Flash Text Engine (FTE), that conserves system memory. However, the FTE is a
low-level API that requires an additional ActionScript 3.0 layer on top of it,
provided in the flash.text.engine package.

For read-only text, it's best to use the Flash Text Engine, which offers low
memory usage and better rendering. For input text, TextField objects are a
better choice, because less ActionScript code is required to create typical
behaviors, such as input handling and word-wrap.

More Help topics

[Rendering text objects](../rendering-performance/rendering-text-objects.md)
