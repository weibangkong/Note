# **Mybatis**

### 1.resultMap与resultType的区别

​	resultType在出现查询出来的数据条目为0时会抛出异常RuntimeException:BindingException

```java
java.lang.RuntimeException: org.apache.ibatis.binding.BindingException: Mapper method 'cn.xxx.TestMapper.test attempted to return null from a method with a primitive return type (int).
```

使用resultMap则不会出现该问题

### 2.脚本对Boolean 类型的值是否为空的判断问题

```xml
<where>
	<if test='null != isTrue and "" != isTrue'>
		and isTure = ${isTrue}
	</if>
</where>
```

上面的写法会造成在isTrue 为false的情况下，不拼接条件语句的问题，默认为isTrue== ""

因此在对Boolean 类型的条件判断是只需要判断其是不是null即可，示例如下

```xml
<where>
	<if test='null != isTrue'>
		and isTure = ${isTrue}
	</if>
</where>
```

### 3.模糊查询

```xml
<script>
	SELECT studentNo FROM student WHERE name LIKE CONCAT('%',#{name},'%')
</script>
```

### 4.遍历拼接条件

```xml
<script>
	INSERT INTO student(id,name,birthday,sex) VALUES
    <foreach collection="list" index="index" item="item" separator=",">
    	(
        	#{item.id},
        	#{item.name},
        	#{item.birthday},
        	#{item.sex}
     	)
    </foreach>
</script>
```

