# Log

1. __Error Log__
    * The error log contains a record of mysqld startup and shutdown times.
      It also contains diagnostic messages such as errors, warnings, and notes that occur during server startup and shutdown, and while the server is running.

2. __General Query Log__
    * The general query log is a general record of what mysqld is doing.
      The server writes information to this log when clients connect or disconnect, and it logs each SQL statement received from clients.
      The general query log can be very useful when you suspect an error in a client and want to know exactly what the client sent to mysqld.

3. __Slow Query Log__
    * The slow query log can be used to find queries that take a long time to execute and are therefore candidates for optimization.
      However, examining a long slow query log can be a time-consuming task.
        + _slow_query_log=1_ -> Enable the slow query
        + _long_query_time_ -> If a query takes longer than this many seconds, it will be written into slow query log
        + _log_queries_not_using_indexes_ -> Queries that are expected to retrieve all rows are logged.
          This option does not necessarily mean that no index is used.
          For example, a query that uses a full index scan uses an index but would be logged because the index would not limit the number of rows.
        + _log_throttle_queries_not_using_indexes_ -> If log_queries_not_using_indexes is enabled,
        the log_throttle_queries_not_using_indexes variable limits the number of such queries per minute that can be written to the slow query log.

4. __Binary Log__
    * The binary log contains "events" that describe database changes such as table creation operations or changes to table data.
      It also contains events for statements that potentially could have made changes (for example, a DELETE which matched no rows).
      The binary log also contains information about how long each statement took that updated data.
      The binary log has two important purposes:
        + _Replication_ -> The source sends the events contained in its binary log to its replicas,
          which execute those events to make the same data changes that were made on the source.
        + _Certain data recovery operations_ -> After a backup has been restored, the events in the binary log that were recorded after the backup was made are re-executed.
          These events bring databases up to date from the point of the backup.

5. __Relay Log__
    * The relay log, like the binary log, consists of a set of numbered files containing events that describe database changes, and an index file that contains the names of all used relay log files.