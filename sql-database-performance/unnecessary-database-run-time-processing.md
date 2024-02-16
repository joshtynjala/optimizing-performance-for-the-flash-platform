# Unnecessary database run-time processing

![](../img/tip_help.png) Use a fully qualified table name (including database
name) in your SQL statement. Always explicitly specify the database name along
with each table name in a statement. (Use "main" if it's the main database). For
example, the following code includes an explicit database name `main`:

    SELECT employeeId
    FROM main.employees

Explicitly specifying the database name prevents the runtime from having to
check each connected database to find the matching table. It also prevents the
possibility of having the runtime choose the wrong database. Follow this rule
even if a SQLConnection is only connected to a single database. Behind the
scenes, the SQLConnection is also connected to a temporary database that is
accessible through SQL statements.

![](../img/tip_help.png) Use explicit column names in SQL `INSERT` and `SELECT`
statements. The following examples show the use of explicit column names:

    INSERT INTO main.employees (firstName, lastName, salary)
    VALUES ("Bob", "Jones", 2000)

    SELECT employeeId, lastName, firstName, salary
    FROM main.employees

Compare the preceding examples to the following ones. Avoid this style of code:

    -- bad because column names aren't specified
    INSERT INTO main.employees
    VALUES ("Bob", "Jones", 2000)

    -- bad because it uses a wildcard
    SELECT *
    FROM main.employees

Without explicit column names, the runtime has to do extra work to figure out
the column names. If a `SELECT` statement uses a wildcard rather than explicit
columns, it causes the runtime to retrieve extra data. This extra data requires
extra processing and creates extra object instances that aren't needed.

![](../img/tip_help.png) Avoid joining the same table multiple times in a
statement, unless you are comparing the table to itself. As SQL statements grow
large, you can unintentionally join a database table to the query multiple
times. Often, the same result could be achieved by using the table only once.
Joining the same table multiple times is likely to happen if you are using one
or more views in a query. For example, you could be joining a table to the
query, and also a view that includes the data from that table. The two
operations would cause more than one join.
