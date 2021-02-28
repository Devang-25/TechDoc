# Basic

1. __Deadlock__
    * When deadlock detection is enabled (the default), InnoDB automatically detects transaction deadlocks and rolls back a transaction or transactions to break the deadlock.
      InnoDB tries to pick small transactions to roll back, where the size of a transaction is determined by the number of rows inserted, updated, or deleted.

2. __Write Ahead Log__
    * The Write Ahead Log (WAL) is one of the most important components of a database.
      All the changes to data files are logged in the WAL (called the redo log in InnoDB).
      This allows to postpone the moment when the modified pages are flushed to disk, still protecting from data losses.
    * Write redo log before flushing dirty page from buffer pool size to disk, to avoid frequent IO operations

3. __TableSpace(Max 64TB) -> Segment -> Extent(1MB [64 pages if page size is 16kb]) -> Page(16kb default) -> Row__

4. __Varchar__
    * It uses one or two bytes to store the number of bytes of the content.
      If it is less than 255 bytes, varchar uses one byte, otherwise, two bytes.
      So the actual bytes=defined bytes + 1|2, for example: varchat(10) = 11, varchat(1000) = 1002