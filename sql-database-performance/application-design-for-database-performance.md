# Application design for database performance

> ![](../img/tip_help.png) Don't change a SQLStatement object's `text` property
> after executing it.

Instead, use one SQLStatement instance for each SQL statement and use statement
parameters to provide different values. Before any SQL statement is executed,
the runtime prepares (compiles) it to determine the steps that are performed
internally to carry out the statement. When you call `SQLStatement.execute()` on
a SQLStatement instance that hasn't executed previously, the statement is
automatically prepared before it is executed. On subsequent calls to the
`execute()` method, as long as the `SQLStatement.text` property hasn't changed
the statement is still prepared. Consequently, it executes faster.

To gain the maximum benefit from reusing statements, if values change between
statement executions, use statement parameters to customize your statement.
(Statement parameters are specified using the `SQLStatement.parameters`
associative array property.) Unlike changing the SQLStatement instance's `text`
property, if you change the values of statement parameters the runtime isn't
required to prepare the statement again.

When you're reusing a SQLStatement instance, your application must store a
reference to the SQLStatement instance once it has been prepared. To keep a
reference to the instance, declare the variable as a class-scope variable rather
than a function-scope variable. One good way to make the SQLStatement a
class-scope variable is to structure your application so that a SQL statement is
wrapped in a single class. A group of statements that are executed in
combination can also be wrapped in a single class. (This technique is known as
using the Command design pattern.) By defining the instances as member variables
of the class, they persist as long as the instance of the wrapper class exists
in the application. At a minimum, you can simply define a variable containing
the SQLStatement instance outside a function so that the instance persists in
memory. For example, declare the SQLStatement instance as a member variable in
an ActionScript class or as a non-function variable in a JavaScript file. You
can then set the statement's parameter values and call its `execute()` method
when you want to actually run the query.

> ![](../img/tip_help.png) Use database indexes to improve execution speed for
> data comparing and sorting.

When you create an index for a column, the database stores a copy of that
column's data. The copy is kept sorted in numeric or alphabetical order. The
sorting allows the database to quickly match values (such as when using the
equality operator) and sort result data using the `ORDER BY` clause.

Database indexes are kept continuously up-to-date, which causes data change
operations (INSERT or UPDATE) on that table to be slightly slower. However, the
increase in data retrieval speed can be significant. Because of this performance
tradeoff, don't simply index every column of every table. Instead, use a
strategy for defining your indexes. Use the following guidelines to plan your
indexing strategy:

- Index columns that are used in joining tables, in WHERE clauses, or ORDER BY
  clauses

- If columns are frequently used together, index them together in a single index

- For a column that contains text data that you retrieve sorted alphabetically,
  specify COLLATE NOCASE collation for the index

> ![](../img/tip_help.png) Consider pre-compiling SQL statements during
> application idle times.

The first time a SQL statement executes, it is slower because the SQL text is
prepared (compiled) by the database engine. Because preparing and executing a
statement can be demanding, one strategy is to preload initial data and then
execute other statements in the background:

1.  Load the data that the application needs first.

2.  When the initial startup operations of your application have completed, or
    at another "idle" time in the application, execute other statements.

For example, suppose your application doesn't access the database at all to
display its initial screen. In that case, wait until that screen displays before
opening the database connection. Finally, create the SQLStatement instances and
execute any that you can.

Alternatively, suppose that when your application starts up it immediately
displays some data, such as the result of a particular query. In that case, go
ahead and execute the SQLStatement instance for that query. After the initial
data is loaded and displayed, create SQLStatement instances for other database
operations and if possible execute other statements that are needed later.

In practice, if you are reusing SQLStatement instances, the additional time
required to prepare the statement is only a one-time cost. It probably doesn't
have a large impact on overall performance.

> ![](../img/tip_help.png) Group multiple SQL data change operations in a
> transaction.

Suppose you're executing many SQL statements that involve adding or changing
data ( `INSERT` or `UPDATE` statements). You can get a significant increase in
performance by executing all the statements within an explicit transaction. If
you don't explicitly begin a transaction, each of the statements runs in its own
automatically created transaction. After each transaction (each statement)
finishes executing, the runtime writes the resulting data to the database file
on the disk.

On the other hand, consider what happens if you explicitly create a transaction
and execute the statements in the context of that transaction. The runtime makes
all the changes in memory, then writes all the changes to the database file at
one time when the transaction is committed. Writing the data to disk is usually
the most time-intensive part of the operation. Consequently, writing to the disk
one time rather than once per SQL statement can improve performance
significantly.

> ![](../img/tip_help.png) Process large `SELECT` query results in parts using
> the SQLStatement class's `execute()` method (with `prefetch` parameter) and
> `next()` method.

Suppose you execute a SQL statement that retrieves a large result set. The
application then processes each row of data in a loop. For example, it formats
the data or creates objects from it. Processing that data can take a large
amount of time, which could cause rendering problems such as a frozen or
non-responsive screen. As described in
[Asynchronous operations](../rendering-performance/asynchronous-operations.md),
one solution is to divide up the work into chunks. The SQL database API makes
dividing up data processing easy to do.

The SQLStatement class's `execute()` method has an optional `prefetch` parameter
(the first parameter). If you provide a value, it specifies the maximum number
of result rows that the database returns when the execution completes:

    dbStatement.addEventListener(SQLEvent.RESULT, resultHandler);
    dbStatement.execute(100); // 100 rows maximum returned in the first set

Once the first set of result data returns, you can call the `next()` method to
continue executing the statement and retrieve another set of result rows. Like
the `execute()` method, the `next()` method accepts a `prefetch` parameter to
specify a maximum number of rows to return:

    // This method is called when the execute() or next() method completes
    function resultHandler(event:SQLEvent):void
    {
        var result:SQLResult = dbStatement.getResult();
        if (result != null)
        {
            var numRows:int = result.data.length;
            for (var i:int = 0; i < numRows; i++)
            {
                // Process the result data
            }

            if (!result.complete)
            {
                dbStatement.next(100);
            }
        }
    }

You can continue calling the `next()` method until all the data loads. As shown
in the previous listing, you can determine when the data has all loaded. Check
the `complete` property of the SQLResult object that's created each time the
`execute()` or `next()` method finishes.

> **Note:** Use the `prefetch` parameter and the `next()` method to divide up
> the processing of result data. Don't use this parameter and method to limit a
> query's results to a portion of its result set. If you only want to retrieve a
> subset of rows in a statement's result set, use the `LIMIT` clause of the
> `SELECT` statement. If the result set is large, you can still use the
> `prefetch` parameter and `next()` method to divide up the processing of the
> results.

> ![](../img/tip_help.png) Consider using multiple asynchronous SQLConnection
> objects with a single database to execute multiple statements simultaneously.

When a SQLConnection object is connected to a database using the `openAsync()`
method, it runs in the background rather than the main runtime execution thread.
In addition, each SQLConnection runs in its own background thread. By using
multiple SQLConnection objects, you can effectively run multiple SQL statements
simultaneously.

There are also potential downsides to this approach. Most importantly, each
additional SQLStatement object requires additional memory. In addition,
simultaneous executions also cause more work for the processor, especially on
machines that only have one CPU or CPU core. Because of these concerns, this
approach is not recommended for use on mobile devices.

An additional concern is that the potential benefit from reusing SQLStatement
objects can be lost because a SQLStatement object is linked to a single
SQLConnection object. Consequently, the SQLStatement object can't be reused if
its associated SQLConnection object is already in use.

If you choose to use multiple SQLConnection objects connected to a single
database, keep in mind that each one executes its statements in its own
transaction. Be sure to account for these separate transactions in any code that
changes data, such as adding, modifying, or deleting data.

Paul Robertson has created an open-source code library that helps you
incorporate the benefits of using multiple SQLConnection objects while
minimizing the potential downsides. The library uses a pool of SQLConnection
objects and manages the associated SQLStatement objects. In this way it ensures
that SQLStatement objects are reused, and multiple SQLConnection objects are
available to execute multiple statements simultaneously. For more information
and to download the library, visit
<https://web.archive.org/web/20140628170011/http://probertson.com/projects/air-sqlite/>.
