[comment]: # (Note: This documentation is generated dynamically in the build process.  To modify the contents, change the javadoc on the _invoke method of the BIF class)

# Function: `QueryExecute`

No description available.

## Method Signature

```
QueryExecute(sql=[String], params=[any], options=[struct])
```

### Arguments


| Argument | Type | Required | Description | Default |
|----------|------|----------|-------------|---------|
| `sql` | `String` | `true` |  |  |
| `params` | `any` | `false` |  | `[]` |
| `options` | `struct` | `false` |  |  |

## Examples

### Simple Example

SQL Only Example. Assumes that a default datasource has been specified (by setting the variable this.datasource in Application.bx)

```java
qryResult = queryExecute("SELECT * FROM Employees");
```

### Passing Query Parameters using Struct

Use `:structKeyName` in your sql then pass a struct with corresponding key names.

```java
qryResult = queryExecute(
  "SELECT * FROM Employees WHERE empid = :empid AND country = :country", 
  {
    country="USA", 
    empid=1
  }
);
```

### Passing Query Parameters using Array

When passing with an array use the `?` symbol as a placeholder in your sql

```java
qryResult = queryExecute(
  "SELECT * FROM Employees WHERE empid = ? AND country = ?", 
  [
    1,
    "USA"
  ]
);
```


## Related

  * [IsInTransaction](./IsInTransaction.md)
  * [IsWithinTransaction](./IsWithinTransaction.md)
  * [PreserveSingleQuotes](./PreserveSingleQuotes.md)
  * [TransactionCommit](./TransactionCommit.md)
  * [TransactionRollback](./TransactionRollback.md)
  * [TransactionSetSavepoint](./TransactionSetSavepoint.md)