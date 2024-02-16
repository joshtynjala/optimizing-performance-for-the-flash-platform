# Perceived performance versus actual performance

The ultimate judges of whether your application performs well are the
application’s users. Developers can measure application performance in terms of
how much time certain operations take to run, or how many instances of objects
are created. However, those metrics aren’t important to end users. Sometimes
users measure performance by different criteria. For example, does the
application operate quickly and smoothly, and respond quickly to input? Does it
have a negative affect on the performance of the system? Ask yourself the
following questions, which are tests of perceived performance:

- Are animations smooth or choppy?

- Does video content look smooth or choppy?

- Do audio clips play continuously, or do they pause and resume?

- Does the window flicker or turn blank during long operations?

- When you type, does the text input keep up or lag behind?

- If you click, does something happen immediately, or is there a delay?

- Does the CPU fan get louder when the application runs?

- On a laptop computer or mobile device, does the battery deplete quickly while
  running the application?

- Do other applications respond poorly when the application is running?

The distinction between perceived performance and actual performance is
important. The way to achieve the best perceived performance isn’t always the
same as the way to get the absolute fastest performance. Make sure that your
application never executes so much code that the runtime isn’t able to
frequently update the screen and gather user input. In some cases, achieving
this balance involves dividing up a program task into parts so that, between
parts, the runtime updates the screen. (See
[Rendering performance](../rendering-performance/index.md) for specific
guidance.)

The tips and techniques described here target improvements in both actual code
execution performance and in how users perceive performance.
