- Type: Search and Replace
- Capturing expression: `^([^,]+),([^,]+).*`
- Replace expression: ``UPDATE `$1` SET `$2` = NEW_ID WHERE `$2` = OLD_ID;``
- Tested in: Visual Studio Code 1.17.1, MariaDB 10.1.26
# Description
This is used as a part of a process to replace all FK references to a row to another row in the same table.

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

The RegEx captures the `TABLE_NAME` and `COLUMN_NAME` and uses them to create an `UPDATE` query.