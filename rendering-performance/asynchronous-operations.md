# Asynchronous operations

![](../img/tip_help.png) Favor using asynchronous versions of operations rather
than synchronous ones, where available. Synchronous operations run as soon as
your code tells them to, and your code waits for them to complete before moving
on. Consequently, they run in the application code phase of the frame loop. If a
synchronous operation takes too long, it stretches the size of the frame loop,
potentially causing the display to appear to freeze or stutter.

When your code executes an asynchronous operation, it doesn’t necessarily run
immediately. Your code and other application code in the current execution
thread continues executing. The runtime then performs the operation as soon as
possible, while attempting to prevent rendering issues. In some cases, the
execution happens in the background and doesn’t run as part of the runtime frame
loop at all. Finally, when the operation completes the runtime dispatches an
event, and your code can listen for that event to perform further work.

Asynchronous operations are scheduled and divided to avoid rendering issues.
Consequently, it is much easier to have a responsive application using
asynchronous versions of operations. See
[Perceived performance versus actual performance](../introduction/perceived-performance-vs-actual-performance.md)
for more information.

However, there is some overhead involved in running operations asynchronously.
The actual execution time can be longer for asynchronous operations, especially
for operations that take a short time to complete.

In the runtime, many operations are inherently synchronous or asynchronous and
you can’t choose how to execute them. However, in Adobe AIR, there are three
types of operations that you can choose to perform synchronously or
asynchronously:

- File and FileStream class operations

  Many operations of the File class can be performed synchronously or
  asynchronously. For example, the methods for copying or deleting a file or
  directory and listing the contents of a directory all have asynchronous
  versions. These methods have the suffix “Async” added to the name of the
  asynchronous version. For example, to delete a file asynchronously, call the
  `File.deleteFileAsync()` method instead of the `File.deleteFile()` method.

  When using a FileStream object to read from or write to a file, the way you
  open the FileStream object determines whether the operations execute
  asynchronously. Use the `FileStream.openAsync()` method for asynchronous
  operations. Data writing is performed asynchronously. Data reading is done in
  chunks, so the data is available one portion at a time. In contrast, in
  synchronous mode the FileStream object reads the entire file before continuing
  with code execution.

- Local SQL database operations

  When working with a local SQL database, all the operations executed through a
  SQLConnection object execute in either synchronous or asynchronous mode. To
  specify that operations execute asynchronously, open the connection to the
  database using the `SQLConnection.openAsync()` method instead of the
  `SQLConnection.open()` method. When database operations run asynchronously,
  they execute in the background. The database engine does not run in the
  runtime frame loop at all, so the database operations are much less likely to
  cause rendering issues.

  For additional strategies for improving performance with the local SQL
  database, see
  [SQL database performance](../sql-database-performance/index.md).

- Pixel Bender standalone shaders

  The ShaderJob class allows you to run an image or set of data through a Pixel
  Bender shader and access the raw result data. By default, when you call the
  `ShaderJob.start()` method the shader executes asynchronously. The execution
  happens in the background, not using the runtime frame loop. To force the
  ShaderJob object to execute synchronously (which is not recommended), pass the
  value `true` to the first parameter of the `start()` method.

In addition to these built-in mechanisms for running code asynchronously, you
can also structure your own code to run asynchronously instead of synchronously.
If you are writing code to perform a potentially long-running task, you can
structure your code so that it executes in parts. Breaking your code into parts
allows the runtime to perform its rendering operations in between your code
execution blocks, making rendering problems less likely.

Several techniques for dividing up your code are listed next. The main idea
behind all these techniques is that your code is written to only perform part of
its work at any one time. You track what the code does and where it stops
working. You use a mechanism such as a Timer object to repeatedly check whether
work remains and perform additional work in chunks until the work is complete.

There are a few established patterns for structuring code to divide up work in
this way. The following articles and code libraries describe these patterns and
provide code to help you implement them in your applications:

- [Asynchronous ActionScript Execution](https://web.archive.org/web/20120509020649/http://www.senocular.com/flash/tutorials/asyncoperations/)
  (article by Trevor McCauley with more background details as well as several
  implementation examples)

- [Parsing & Rendering Lots of Data in Flash Player](https://web.archive.org/web/20150601191847/http://jessewarden.com/2009/02/parsing-rendering-lots-of-data-in-flash-player.html)
  (article by Jesse Warden with background details and examples of two
  approaches, the “builder pattern” and “green threads”)

- [Green Threads](https://web.archive.org/web/20121031135308/http://blog.generalrelativity.org/actionscript-30/green-threads/)
  (article by Drew Cummins describing the “green threads” technique with example
  source code)

- [greenthreads](https://code.google.com/archive/p/greenthreads/) (open-source
  code library by Charlie Hubbard, for implementing “green threads” in
  ActionScript. See Actionscript and Concurrency
  [Part 1](https://web.archive.org/web/20150504000342/http://wrongnotes.blogspot.com/2009/02/concurrency-and-actionscript-part-i-of.html),
  [Part 2](https://web.archive.org/web/20150503235745/http://wrongnotes.blogspot.com/2009/02/actionscript-and-concurrency-ii-of-iii.html),
  and
  [Part 3](https://web.archive.org/web/20160208223856/http://wrongnotes.blogspot.com/2009/02/actionscript-and-concurrency-iii-of-iii.html)
  for more information.)

- Threads in ActionScript 3 at
  <https://web.archive.org/web/20100604234338/http://blogs.adobe.com/aharui/2008/01/threads_in_actionscript_3.html>
  (article by Alex Harui, including an example implementation of the “pseudo
  threading” technique)
