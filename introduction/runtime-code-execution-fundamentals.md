# Runtime code execution fundamentals

One key to understanding how to improve application performance is to understand
how the Flash Platform runtime executes code. The runtime operates in a loop
with certain actions occurring each “frame.” A frame in this case is simply a
block of time determined by the frame rate specified for the application. The
amount of time allotted to each frame directly corresponds to the frame rate.
For example, if you specify a frame rate of 30 frames per second, the runtime
attempts to make each frame last one-thirtieth of a second.

You specify the initial frame rate for your application at authoring time. You
can set the frame rate using settings in Adobe® Flash® Builder™ or Flash
Professional. You can also specify the initial frame rate in code. Set the frame
rate in an ActionScript-only application by applying the `[SWF(frameRate="24")]`
metadata tag to your root document class. In MXML, set the `frameRate` attribute
in the Application or WindowedApplication tag.

Each frame loop consists of two phases, divided into three parts: events, the
`enterFrame` event, and rendering.

The first phase includes two parts (events and the `enterFrame` event), both of
which potentially result in your code being called. In the first part of the
first phase, runtime events arrive and are dispatched. These events can
represent completion or progress of asynchronous operations, such as a response
from loading data over a network. They also include events from user input. As
events are dispatched, the runtime executes your code in listeners you’ve
registered. If no events occur, the runtime waits to complete this execution
phase without performing any action. The runtime never speeds up the frame rate
due to lack of activity. If events occur during other parts of the execution
cycle, the runtime queues up those events and dispatches them in the next frame.

The second part of the first phase is the `enterFrame` event. This event is
distinct from the others because it is always dispatched once per frame.

Once all the events are dispatched, the rendering phase of the frame loop
begins. At that point the runtime calculates the state of all visible elements
on the screen and draws them to the screen. Then the process repeats itself,
like a runner going around a racetrack. Note: For events that include an
`updateAfterEvent` property, rendering can be forced to occur immediately
instead of waiting for the rendering phase. However, avoid using
`updateAfterEvent` if it frequently leads to performance problems. It's easiest
to imagine that the two phases in the frame loop take equal amounts of time. In
that case, during half of each frame loop event handlers and application code
are running, and during the other half, rendering occurs. However, the reality
is often different. Sometimes application code takes more than half the
available time in the frame, stretching its time allotment, and reducing the
allotment available for rendering. In other cases, especially with complex
visual content such as filters and blend modes, the rendering requires more than
half the frame time. Because the actual time taken by the phases is flexible,
the frame loop is commonly known as the “elastic racetrack.”

If the combined operations of the frame loop (code execution and rendering) take
too long, the runtime isn’t able to maintain the frame rate. The frame expands,
taking longer than its allotted time, so there is a delay before the next frame
is triggered. For example, if a frame loop takes longer than one-thirtieth of a
second, the runtime is not able to update the screen at 30 frames per second.
When the frame rate slows, the experience degrades. At best animation becomes
choppy. In worse cases, the application freezes and the window goes blank.

For more details about the Flash Platform runtime code execution and rendering
model, see the following resources:

- [Flash Player Mental Model - The Elastic Racetrack](https://web.archive.org/web/20140316000435/http://tedpatrick.com/2005/07/19/flash-player-mental-model-the-elastic-racetrack/)
  (article by Ted Patrick)

- [Updated 'Elastic Racetrack' for Flash 9 and AVM2](https://web.archive.org/web/20230321111648/https://www.craftymind.com/updated-elastic-racetrack-for-flash-9-and-avm2/)
  (article by Sean Christmann)

- [Asynchronous ActionScript Execution](https://web.archive.org/web/20120509020649/http://www.senocular.com/flash/tutorials/asyncoperations/)
  (article by Trevor McCauley)

- Optimizing Adobe AIR for code execution, memory & rendering at
  <https://web.archive.org/web/20140218170500/http://tv.adobe.com/watch/max-2008-develop/optimizing-adobe-air-for-code-execution-memory-rendering/>
  (Video of MAX conference presentation by Sean Christmann)
  [(PDF version)](https://web.archive.org/web/20220121181111/http://www.craftymind.com/wordpress/wp-content/uploads/2008/11/sean_christmann_optimizing_air_final.pdf)
