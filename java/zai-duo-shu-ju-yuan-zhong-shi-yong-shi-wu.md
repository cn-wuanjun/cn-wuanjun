---
description: 事务注解`@Transactional`
---

# 在多数据源中使用事务

在常规项目中直接在方法或者类上使用注解即可

但如果项目配置了多数据源的情况,会出现不回滚的现象,这是因为Transactional会默认指定一个`DataSourceTransactionManager`.&#x20;

## 如何解决?

在注解中有指定DataSource的属性字段.

```java
@AliasFor("transactionManager")
String value() default "";

@AliasFor("value")
String transactionManager() default "";
```

这里2个参数是等效的.

具体用法:

```java
@Transactional(rollbackFor = Exception.class, transactionManager = "insTransactionManager")
```
