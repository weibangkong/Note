# Spring Boot

## 1.配置

#### 1.context-path：

要么不指定(推荐不指定)，指定的话一定要以 “/” 开头；不推荐配置是因为使用Jenkins+Rancher持续部署时可以在Rancher中指定

#### 2.@EnableWebMvc

使用该注解会使boot关于mvc部分的自动配置完全失效，关于mvc部分的配置需用户全部手动实现

原理：@EnableWebMvc是一个复合注解，该注解定义上又一个@Import，引入了一个DelegatingWebMvcConfiguration类，这个类继承了WebMvcConfigurationSupport类,而负责SpringBoot MVC自动配置的类WebMvcAutoConfiguration上有一个@ConditionOnMissingBean(WebMvcConfiguronationSupport.class),即只有在容器中没有发现WebMvcConfiguronationSupport类的时候，才进行自动配置，当使用@EnableWebMvc注解时，就引入了一个WebMvcConfiguronationSupport类，因此自动配置失效，由用户完全负责管理

#### 3.文件上传问题:

报错:

```java
Unable to process parts as no multi-part configuration has been provided
```

原因及排错:

##### 1:如未自定义ServletRegistrationBean:

如果没有过可以在application中添加如下配置：

**较新版本的Spring Boot**

```yaml
spring:
	servlet:
		multipart:
			max-file-size: 10MB
			max-request-size: 10MB
```

或

**较旧版本的Spring Boot**

```yaml
spring:
	http:
		multipart:
			max-file-size: 10MB
			max-request-size: 10MB
```

***该配置还可以解决上传下载文件大小的限制***

##### 2.如有自己配置ServeleRegistrationBean：

```java
// 使用Spring 默认提供的多部件配置
@Autowired
private MultipartConfigElement multipartConfigElement;

@Bean
public ServletRegistrationBean dispatcherRegistration(DispatcherServlet dispatcherServlet) {
	ServletRegistrationBean servletRegistration = new ServletRegistrationBean(dispatcherServlet);
	servletRegistration.setMultipartConfig(multipartConfigElement);
	return servletRegistration;
}
```

#### 4.文件上传时还要附带额外参数

报错如下:

```java
Content type 'multipart/form-data;boundary=----WebKitFormBoundaryJAAjJA4hgL1Jvgan;charset=UTF-8' not supported
```

解决方案:

将后台上传控制器对应方法中参数的@RequestBody注解去掉