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