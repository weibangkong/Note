# **SpringMVC**

### 1.注解使用

#### 1.@RequestParam

只可以获取到url中的参数，在一个api方法中可以多次出现

#### 2.@RequestBody

只可以获取到body中的参数，在一个api方法中仅可出现一次

#### 3.@PathVariable

效果类似@RequestParam,但是是根据下标来绑定参数，在一个api方法中可以多次出现，在元素为null的时候可能出现问题，不推荐使用

#### 4.报错XXX及其超类对上下文都是未知类型：

指定返回的数据格式，使用的HttpMessageConverter是否支持转换成多种数据格式

#### 5.做Post文件上传时，在上传文件的同时还需要上传参数，报错“ContextType:Multipart/form-dataxxxx” xxx not support

原因:使用了@RequestBody注解,将@RequestBody注解去掉即可

#### 6.put请求的参数问题：

put请求时，参数是放在url中的

#### 7.转发服务，响应中的中文乱码:

##### 1.检查方法

```java
@RequestMapping(produces=MediaType.APPLLICATION_JSON_UTF8_VALUE)
```

##### 2.检查配置文件中设置

```yaml
spring.http.encoding.charset: UTF-8
新版本中的springboot
spring.servlet.encoding.charset: UTF-8
```

##### 3.检查是不是使用response直接往外write么，如果是的话：

```java
response.setContentType(MediaType.APPLICATION_JSON_VALUE);
response.getWriter().write(Object);
```

