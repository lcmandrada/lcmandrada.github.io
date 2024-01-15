# SQL

## LiquiBase

### Release changelock
```sql
update databasechangeloglock set locked=false, lockgranted=null, lockedby=null where id=1;
select * from databasechangeloglock;
```
