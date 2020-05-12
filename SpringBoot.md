# Spring Boot

## 1.配置

#### 1.context-path：

要么不指定(推荐不指定)，指定的话一定要以 “/” 开头；不推荐配置是因为使用Jenkins+Rancher持续部署时可以在Rancher中指定

#### 2.@EnableWebMvc

使用该注解会使boot关于mvc部分的自动配置完全失效，关于mvc部分的配置需用户全部手动实现

原理：@EnableWebMvc是一个复合注解，该注解定义上又一个@Import，引入了一个DelegatingWebMvcConfiguration类，这个类继承了WebMvcConfigurationSupport类,而负责SpringBoot MVC自动配置的类WebMvcAutoConfiguration上有一个@ConditionOnMissingBean(WebMvcConfiguronationSupport.class),即只有在容器中没有发现WebMvcConfiguronationSupport类的时候，才进行自动配置，当使用@EnableWebMvc注解时，就引入了一个WebMvcConfiguronationSupport类，因此自动配置失效，由用户完全负责管理

