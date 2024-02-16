# Target your optimizations

Some performance improvements do not create a noticeable improvement for users.
It’s important to concentrate your performance optimizations on areas that are
problems for your specific application. Some performance optimizations are
general good practices and can always be followed. For other optimizations,
whether they are useful depends on your application’s needs and its anticipated
user base. For example, applications always perform better if you don’t use any
animation, video, or graphic filters and effects. However, one of the reasons
for using the Flash Platform to build applications is because of the media and
graphics capabilities that allow rich expressive applications. Consider whether
your desired level of richness is a good match for the performance
characteristics of the machines and devices on which your application runs.

One common piece of advice is to “avoid optimizing too early.” Some performance
optimizations require writing code in a way that is harder to read or less
flexible. Such code, once optimized, is more difficult to maintain. For these
optimizations, it is often better to wait and determine whether a particular
section of code performs poorly before choosing to optimize the code.

Improving performance sometimes involves making trade-offs. Ideally, reducing
the amount of memory consumed by an application also increases the speed at
which the application performs a task. However, that type of ideal improvement
isn’t always possible. For example, if an application freezes during an
operation, the solution often involves dividing up work to run over multiple
frames. Because the work is being divided up, it is likely to take longer
overall to accomplish the process. However, it is possible for the user to not
notice the additional time, if the application continues to respond to input and
doesn’t freeze.

One key to knowing what to optimize, and whether optimizations are helpful, is
to conduct performance tests. Several techniques and tips for testing
performance are described in
[Benchmarking and deploying](../benchmarking-and-deploying/index.md).

For more information about determining parts of an application that are good
candidates for optimization, see the following resources:

- Performance-tuning apps for AIR at
  <http://www.adobe.com/go/learn_fp_goldman_tv_en> (Video of MAX conference
  presentation by Oliver Goldman)

- Performance-tuning Adobe AIR applications at
  <http://www.adobe.com/go/learn_fp_air_perf_devnet_en> (Adobe Developer
  Connection article by Oliver Goldman, based on the presentation)
