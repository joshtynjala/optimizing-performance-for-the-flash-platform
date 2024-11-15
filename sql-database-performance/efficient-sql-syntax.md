# Efficient SQL syntax

> ![](../img/tip_help.png) Use `JOIN` (in the `FROM` clause) to include a table
> in a query instead of a subquery in the `WHERE` clause.

This tip applies even if you only need a table's data for filtering, not for the
result set. Joining multiple tables in the `FROM` clause performs better than
using a subquery in a `WHERE` clause.

> ![](../img/tip_help.png) Avoid SQL statements that can't take advantage of
> indexes.

These statements include the use of aggregate functions in a subquery, a `UNION`
statement in a subquery, or an `ORDER BY` clause with a `UNION` statement. An
index can greatly increase the speed of processing a `SELECT` query. However,
certain SQL syntax prevents the database from using indexes, forcing it to use
the actual data for searching or sorting operations.

> ![](../img/tip_help.png) Consider avoiding the `LIKE` operator, especially
> with a leading wildcard character as in `LIKE('%XXXX%')`.

Because the `LIKE` operation supports the use of wildcard searches, it performs
slower than using exact-match comparisons. In particular, if you start the
search string with a wildcard character, the database can't use indexes at all
in the search. Instead, the database must search the full text of each row of
the table.

> ![](../img/tip_help.png) Consider avoiding the `IN` operator.

If the possible values are known beforehand, the `IN` operation can be written
using `AND` or `OR` for faster execution. The second of the following two
statements executes faster. It is faster because it uses simple equality
expressions combined with `OR` instead of using the `IN()` or `NOT IN()`
statements:

    -- Slower
    SELECT lastName, firstName, salary
    FROM main.employees
    WHERE salary IN (2000, 2500)

    -- Faster
    SELECT lastName, firstName, salary
    FROM main.employees
    WHERE salary = 2000
    OR salary = 2500

> ![](../img/tip_help.png) Consider alternative forms of a SQL statement to
> improve performance.

As demonstrated by previous examples, the way a SQL statement is written can
also affect database performance. There are often multiple ways to write a SQL
`SELECT` statement to retrieve a particular result set. In some cases, one
approach runs notably faster than another one. In addition to the preceding
suggestions, you can learn more about different SQL statements and their
performance from dedicated resources on the SQL language.
