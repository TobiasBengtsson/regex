# Delete all references to a table row
- Type: Search and Replace
- Capturing expression: `^([^,]+),([^,]+).*`
- Replace expression: ``DELETE FROM `$1` WHERE `$2` = ID_TO_DELETE;``
- Tested in: Visual Studio Code 1.17.1, MariaDB 10.1.26
- See also: [Replace foreign key to table](./replace_fk_to_table.md)
# Description
This is used as a part of a process to delete all FK references to a row in a table.

Start by selecting all tables referencing the table by running

```
SELECT
  TABLE_NAME,COLUMN_NAME,CONSTRAINT_NAME, REFERENCED_TABLE_NAME,REFERENCED_COLUMN_NAME
FROM
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
  REFERENCED_TABLE_SCHEMA = 'database' AND
  REFERENCED_TABLE_NAME = 'table';
```

Then you will get a list of tables that reference the table you specify. An example row would be:

TABLE_NAME,COLUMN_NAME,FK_NAME,REFERENCED_TABLE_NAME,REFERENCED_COLUMN_NAME

The RegEx captures the `TABLE_NAME` and `COLUMN_NAME` and uses them to create an `DELETE` query.
