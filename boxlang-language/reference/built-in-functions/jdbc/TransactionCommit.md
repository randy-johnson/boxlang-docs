[comment]: # (Note: This documentation is generated dynamically in the build process.  To modify the contents, change the javadoc on the _invoke method of the BIF class)

# Function: `TransactionCommit`

No description available.

## Method Signature

```
TransactionCommit()
```

### Arguments

This function does not accept any arguments

## Examples

### Using TransactionCommit with TransactionRollback

This simple example shows two JDBC queries executed inside a transaction. The first query, an INSERT, is committed to the database while the second is rolled back.

```java
transaction {
    queryExecute( "INSERT INTO vehicles (id,make) VALUES (8, 'Ford' )", {}, { datasource : "carDB" } );
    transactionCommit();

    queryExecute( "UPDATE developers SET name = 'Chevrolet' WHERE id=8", {}, { datasource : "carDB" } );
    transactionRollback();
}
```

Note that the rollback cannot affect the INSERT statement, since it has already been committed ("persisted") to the database.

## Related

  * [IsInTransaction](./IsInTransaction.md)
  * [IsWithinTransaction](./IsWithinTransaction.md)
  * [PreserveSingleQuotes](./PreserveSingleQuotes.md)
  * [QueryExecute](./QueryExecute.md)
  * [TransactionRollback](./TransactionRollback.md)
  * [TransactionSetSavepoint](./TransactionSetSavepoint.md)