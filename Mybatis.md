# **Mybatis**

### 1.resultMap与resultType的区别

​	resultType在出现查询出来的数据条目为0时会抛出异常RuntimeException:BindingException

```java
java.lang.RuntimeException: org.apache.ibatis.binding.BindingException: Mapper method 'cn.xxx.TestMapper.test attempted to return null from a method with a primitive return type (int).
```

使用resultMap则不会出现该问题