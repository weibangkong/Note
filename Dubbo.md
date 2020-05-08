# **DUBBO**

## 1.基本使用

### 1.服务端:

##### 1.配置文件中要添加相关配置

```yaml
dubbo:
 application:
  name: blood-service
 protocal:
  name: dubbo
  port: 21880
 registry:
  address: zookeeper://ip:port:2181
 
```



##### 2.启动类或者配置类上添加@EnableDubbo

##### 3.要提供服务的类添加@org.apache.dubbo.config.annotion.Service



### 2.调用端(客户端)

##### 1.配置文件中添加相关依赖

```yaml
dubbo:
 application:
  name: need-service
 protocal:
  name: dubbo
  port: 21880
 registry:
  address: zookeeper://ip:port:2181
```

##### 2.启动类或者配置类上添加@EnableDubbo(***也可以不加,不加也能调通***)

##### 3.引入要服务端api

##### 4.使用@org.apache.dubbo.config.annotion.Reference将需要调用的远程服务注入进来





## 2.使用经验:

**1.异常**：如果是客户端调用了服务端的方法，在服务端内发生异常，异常将会被自动向上传递给调用端(异常会从服务端抛到调用端)





## @EnableDubbo注解作用: