# DB Design

1. __Break your data into logical pieces__
    * If your queries are using too many string parsing functions like substring, charindex, etc., then probably this rule needs to be applied.
      Common example, Name and Address field. Break this field into further logical pieces so that we can write clean and optimal queries.

2. __Avoid data with separators__
    * Data stuffed with separators need special attention and should be to moved to a different table. Link them with keys for better management.

3. __Self-reference for hierarchical data__
    * Data with unlimited parent child hierarchy. Consider a sales person can have multiple sales people below them.
    * Using a self-referencing primary key and foreign key will simplify database design.

4. __Keep passwords as encryted or hashed for security. Decrypt them in application when required__

5. __Use integer id fields for all tables. If id is not required for the time being, it may be required in then future__

6. __Choose columns with the integer data type for indexing. Varchar column indexing will cause performance problems__

7. __Use indexes for frequently used queries on big tables__

8. __Place image and blob data in separate tables__