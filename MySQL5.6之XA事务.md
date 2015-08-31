MySQL5.6之XA事务
================

XA事务SQL语句
-------------

```sql
XA {START|BEGIN} xid [JOIN|RESUME]

XA END xid [SUSPEND [FOR MIGRATE]]

XA PREPARE xid

XA COMMIT xid [ONE PHASE]

XA ROLLBACK xid

XA RECOVER
```

**特别说明**  
1. 对于XA START语句,JOIN/RESUME语句不支持;对于XA END语句,SUSPEND [FOR MIGRATE]语句并不支持。