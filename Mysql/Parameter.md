# Parameter

1. __log_output__
    * Desciption : The destination or destinations for general query log and slow query log output.
      The value is a list one or more comma-separated words chosen from TABLE, FILE, and NONE.
      TABLE selects logging to the general_log and slow_log tables in the mysql system database.
    * Scope : Global
    * Default Value : FILE
    * Valid Values : TABLE & FILE & NONE

2. __innodb_fast_shutdown__
    * Desciption : The InnoDB shutdown mode. The slow shutdown can take minutes,
      or even hours in extreme cases where substantial amounts of data are still buffered.
      Use the slow shutdown technique before upgrading or downgrading between MySQL major releases,
      so that all data files are fully prepared in case the upgrade process updates the file format.
    * Scope : Global
    * Default Value : 1
    * Valid Values : 0 | 1 | 2
        + 0 -> InnoDB does a slow shutdown, a full purge and a change buffer merge before shutting down.
        + 1 -> InnoDB skips these operations at shutdown, a process known as a fast shutdown.
        + 2 -> InnoDB flushes its logs and shuts down cold, as if MySQL had crashed. no committed transactions are lost.

3. __innodb_force_recovery__
    * Desciption : The crash recovery mode, typically only changed in serious troubleshooting situations.
    * Scope : Global
    * Default Value : 0
    * Valid Values : 0 | 1 | 2 | 3 | 4 | 5 | 6
        + 0 -> Normal startup without forced recovery.
        + 1 -> Lets the server run even if it detects a corrupt page.
        + 2 -> Prevents the master thread and any purge threads from running. If an unexpected exit would occur during the purge operation, this recovery value prevents it.
        + 3 -> Does not run transaction rollbacks after crash recovery.
        + 4 -> Prevents insert buffer merge operations. If they would cause a crash, does not do them.
        + 5 -> Does not look at undo logs when starting the database.
        + 6 -> Does not do the redo log roll-forward in connection with recovery. This value can permanently corrupt data files.

4. __innodb-flush-neighbors__
    * Desciption : Specifies whether flushing a page from the InnoDB buffer pool also flushes other dirty pages in the same extent.
    * Scope : Global
    * Default Value : 1
    * Valid Values : 0 | 1 | 2
        + 0 -> Disable.
        + 1 -> Flush contiguous dirty pages in the same extent.
        + 2 -> Flush dirty pages in the same extent.