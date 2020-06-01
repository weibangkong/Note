# **Mybatis**

### 1.resultMap与resultType的区别

​	resultType在出现查询出来的数据条目为0时会抛出异常RuntimeException:BindingException

```java
java.lang.RuntimeException: org.apache.ibatis.binding.BindingException: Mapper method 'cn.xxx.TestMapper.test attempted to return null from a method with a primitive return type (int).
```

使用resultMap则不会出现该问题

造成该问题的另外的原因是，如果resultType使用的是基本类型的包装类型，那么在调用的方法接收返回值的时候如果使用的是基本类型接受，如果查出来的是null,同样会抛出该异常

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

### 5.不生成xml文件(即不指定数据库字段到类属性的映射),在字段名结果名不一致的情况下，查询出来字段结果名为null的情况

```yaml
myabtis：
	configuration：
		map-underscore-to-camel-case： true
```

##### 这种情况形成的原因就是没有指定映射，即数据库字段名和类属性不一致

### 6.如果配置了映射但是，sql查询出来有数据但是却无法自动封装到实体中

形成这种情况的原因可能是因为在项目中配置了多数据源，如果配置了多数据源，只需要多数据源的SqlSessionFactory配置上configuration即可，示例如下

```java
@Bean
public SqlSessionFactory getSqlSessionFactory(DataSource datasource, org.apache.ibatis.session.Configuration config){
    SqlSessionFactoryBean sqlSessionFactory = new SqlSessionFacotryBean(datasource);
    sqlSessionFactory.setConfiguration(config);
    return sqlSessionFactory.getObject();
}
```



