# Database file optimization

![](../img/tip_help.png) Avoid database schema changes. If possible, avoid
changing the schema (table structure) of a database once you’ve added data into
the database’s tables. Normally a database file is structured with the table
definitions at the start of the file. When you open a connection to a database,
the runtime loads those definitions. When you add data to database tables, that
data is added to the file after the table definition data. However, if you make
schema changes, the new table definition data is mixed in with the table data in
the database file. For example, adding a column to a table or adding a new table
can result in the mixing of types of data. If the table definition data is not
all located at the beginning of the database file, it takes longer to open a
connection to the database. The connection is slower to open because it takes
the runtime longer to read the table definition data from different parts of the
file. ![](../img/tip_help.png) Use the `SQLConnection.compact()` method to
optimize a database after schema changes. If you must make schema changes, you
can call the `SQLConnection.compact()` method after completing the changes. This
operation restructures the database file so that the table definition data is
located together at the start of the file. However, the `compact()` operation
can be time-intensive, especially as a database file grows larger.
