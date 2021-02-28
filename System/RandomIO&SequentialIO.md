# Random I/O And Sequential I/O

1. __Random I/O__
    * Disk head reads data scattered on various parts of disk.
      Different tracks are read that are not adjacent, so random movement of head causes degrade in performance.

2. __Sequential I/O__
    * It is operation in which adjacent data is accessed on a disk drive.
      Track to track seek is performed and thus greater throughput is provided.

> Keeping in view the major characteristic of sequential I/O and random I/O, it is important to design database disk storage in such a way that maximum sequential I/O may be performed.
  Transactional log is a major example of sequential I/O in SQL Server environment. You should place a heavily used log file on one disk.
  And it is good approach to use just only one default log file. If you place multiple log files on same disk the access type will be changed from sequential to random I/O. 