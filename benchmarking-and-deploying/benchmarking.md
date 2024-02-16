# Benchmarking

There are a number of tools available for benchmarking applications. You can use
the Stats class and the PerformanceTest class, developed by Flash community
members. You can also use the profiler in Adobe® Flash® Builder™, and the
FlexPMD tool.

## The Stats class

To profile your code at runtime using the release version of the runtime,
without an external tool, you can use the Stats class developed by mr. doob from
the Flash community. You can download the Stats class at the following address:
<https://github.com/mrdoob/Hi-ReS-Stats>. The Stats class allows you to track
the following things:

- Frames rendered per second (the higher the number, the better).

- Milliseconds used to render a frame (the lower number, the better).

- The amount of memory the code is using. If it increases on each frame, it is
  possible that your application has a memory leak. It is important to
  investigate the possible memory leak.

- The maximum amount of memory the application used. Once downloaded, the Stats
  class can be used with the following compact code:

      import net.hires.debug.*;
      addChild( new Stats() );

By using conditional compilation in Adobe® Flash® Professional or Flash Builder,
you can enable the Stats object:

    CONFIG::DEBUG
    {
      import net.hires.debug.*;
      addChild( new Stats() );
    }

By switching the value of the `DEBUG` constant, you can enable or disable the
compilation of the Stats object. The same approach can be used to replace any
code logic that you do not want to be compiled in your application.

## The PerformanceTest class

To profile ActionScript code execution, Grant Skinner has developed a tool that
can be integrated into a unit testing workflow. You pass a custom class to the
PerformanceTest class, which performs a series of tests on your code. The
PerformanceTest class allows you to benchmark different approaches easily. The
PerformanceTest class can be downloaded at the following address:
<https://web.archive.org/web/20110309164943/http://www.gskinner.com/blog/archives/2009/04/as3_performance.html>.

## Flash Builder profiler

Flash Builder is shipped with a profiler that allows you to benchmark your code
with a high level of detail. Note: Use the debugger version of Flash Player to
access the profiler, or you'll get an error message. The profiler can also be
used with content produced in Adobe Flash Professional. To do that, load the
compiled SWF file from an ActionScript or Flex project into Flash Builder, and
you can run the profiler on it. For more information on the profiler, see
"Profiling Flex applications" in
[Using Flash Builder 4](https://web.archive.org/web/20150219140156/http://help.adobe.com/en_US/Flex/4.0/UsingFlashBuilder/index.html).

## FlexPMD

Adobe Technical Services has released a tool called FlexPMD, which allows you to
audit the quality of ActionScript 3.0 code. FlexPMD is an ActionScript tool,
which is similar to JavaPMD. FlexPMD improves code quality by auditing an
ActionScript 3.0 or Flex source directory. It detects poor coding practices,
such as unused code, overly complex code, overly lengthy code, and incorrect use
of the Flex component life cycle.

FlexPMD is an Apache open-source project available at the following address:
<https://github.com/apache/flex-utilities/tree/develop/FlexPMD>.

FlexPMD makes it easier to audit code and to make sure that your code is clean
and optimized. The real power of FlexPMD lies in its extensibility. As a
developer, you can create your own sets of rules to audit any code. For example,
you can create a set of rules that detect heavy use of filters, or any other
poor coding practice that you want to catch.
