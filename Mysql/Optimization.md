1. Don't use multi storage engines in one transaction.
   Because when we use InnoDB and MyISAM together, if transaction failed, it's difficult to rollback MyISAM table.

2. Use BENCHMARK to test performance of the function.

3. Use the shortest data type which can store your data completely.

4. Avoid use null, it comsumes more computing resource to search and is impossible to create index on it.

5. Use int to instead of string as far as possible, comparing int is more efficient than string, due to there are many different collations.

6. Use enum instead of string, if values are constant.
   _Note:_ Order by doesn't use alphabetical order, but the order when it's defined.
   Due to enum is stored as integer behind, so don't define integer enum.

7. Use unsigned number type for positive only number, to have doule capacity to store content.

8. Use decimal or int instead of float or double, to gain high precision.

9. Use LIMIT 1 expression when we need only one record

10. When using join, make sure the indexes exist for both columns, and those columns are in same type

11.