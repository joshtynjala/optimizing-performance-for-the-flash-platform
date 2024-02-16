# Unnecessary network operations

![](../img/tip_help.png) Cache assets locally after loading them, instead of
loading them from the network each time they're needed. If your application
loads assets such as media or data, cache the assets by saving them to the local
device. For assets that change infrequently, consider updating the cache at
intervals. For example, your application could check for a new version of an
image file once per day, or check for fresh data once every two hours.

You can cache assets in several ways, depending on the type and nature of the
asset:

- Media assets such as images and video: save the files to the file system using
  the File and FileStream classes

- Individual data values or small sets of data: save the values as local shared
  objects using the SharedObject class

- Larger sets of data: save the data in a local database, or serialize the data
  and save it to a file

For caching data values, the
[open-source AS3CoreLib project](https://github.com/mikechambers/as3corelib)
includes a ResourceCache class that does the loading and caching work for you.
