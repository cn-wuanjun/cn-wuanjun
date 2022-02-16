# 多参数传递



**索引方式**

_**通过索引方式,来指定想传入的参数,`#{index}`,索引从0开始:**_ DAO:

```
int get(String a,int b);
```

XML:

```
<select id="get" resultType="int">
    select id from table 
    where a=#{0} and b=#{1}
</select>
```

注意： 1.由于是多参数传入，所以不需要对`parameterType`进行配置。 2.由于使用索引方式，所以在`DAO`接口中不需要使用`@Param`注解来注明参数名

**注解方式**

_**通过MyBatis的注解(`@Param（"paramName"）`)方式来注明参数**_ DAO:

```
int get(@Param("a")String a,@Param("b")int b);
```

XML:

```
<select id="get" resultType="int">
    select id from table 
    where a=#{a} and b=#{b}
</select>
```

注意： 1.同样由于是多参数传入，所以不需要对parameterType进行配置。

**Map对象方式**

_**通过Map方式传递多个参数，map中的key的名字就是对应xml配置中#{}中使用的那个**_ DAO:

```
int get(Map map);
```

XML:

```
<select id="get" resultType="int">
    select id from table 
    where a=#{a} and b=#{b}
</select>
```
