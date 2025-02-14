[comment]: # (Note: This documentation is generated dynamically in the build process.  To modify the contents, change the javadoc on the _invoke method of the Component class)
# Component: `DBInfo`

Retrieve database metadata for a given datasource.

## Component Signature

```
<bx:DBInfo type=[string]
name=[string]
datasource=[string]
table=[string]
pattern=[string]
dbname=[string]
filter=[string]
username=[string]
password=[string] />
```

### Attributes


| Atrribute | Type | Required | Description | Default |
|----------|------|----------|-------------|---------|
| `type` | `string` | `true` | Type of metadata to retrieve. One of: `columns`, `dbnames`, `tables`, `foreignkeys`, `index`, `procedures`, or `version`. |  |
| `name` | `string` | `true` | Name of the variable to which the result will be assigned. Required. |  |
| `datasource` | `string` | `false` | Name of the datasource to check metadata on. If not provided, the default datasource will be used. |  |
| `table` | `string` | `false` | Table name for which to retrieve metadata. Required for `columns`, `foreignkeys`, and `index` types. |  |
| `pattern` | `string` | `false` |  |  |
| `dbname` | `string` | `false` |  |  |
| `filter` | `string` | `false` | This is a string value that must match a table type in your database implementation. Each database is different.<br>                   Some common filter types are:<br>                   <ul><br>                   <li>TABLE - This is the default value and will return only tables.</li><br>                   <li>VIEW - This will return only views.</li><br>                   <li>SYSTEM TABLE - This will return only system tables.</li><br>                   <li>GLOBAL TEMPORARY - This will return only global temporary tables.</li><br>                   <li>LOCAL TEMPORARY - This will return only local temporary tables.</li><br>                   <li>ALIAS - This will return only aliases.</li><br>                   <li>SYNONYM - This will return only synonyms.</li><br>                   </ul> |  |
| `username` | `string` | `false` |  |  |
| `password` | `string` | `false` |  |  |

## Examples

```
<bx:DBInfo type=[string]
name=[string]
datasource=[string]
table=[string]
pattern=[string]
dbname=[string]
filter=[string]
username=[string]
password=[string] />
```