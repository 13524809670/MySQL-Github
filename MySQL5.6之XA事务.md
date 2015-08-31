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
1. 对于XA START语句,JOIN/RESUME语句不支持;对于XA END语句,SUSPEND [FOR MIGRATE]语句并不支持;  
2. 每一个XA语句都是以XA开头的;  
3. xid的值是有客户提供的或者有MySQL服务器生成;  
4. xid的组成如下  

```sql
xid: gtrid [, bqual [, formatID ]]
```
*girid*是全局事务ID,64字节的字符串;  
*bqual*是分支(branch)的修饰符,64字节的字符串;  
*formatID*是用于标识gtrid和bqual的格式,无符号的整数数值;  
xid是由transaction manager管理;  
每一个XA事务都必须拥有其唯一的xid值;  

XA事务state
-----------



