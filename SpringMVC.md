# **SpringMVC**

### 1.注解使用

#### 1.@RequestParam

只可以获取到url中的参数，在一个api方法中可以多次出现

#### 2.@RequestBody

只可以获取到body中的参数，在一个api方法中仅可出现一次

#### 3.@PathVariable

效果类似@RequestParam,但是是根据下标来绑定参数，在一个api方法中可以多次出现，在元素为null的时候可能出现问题，不推荐使用



4.做Post文件上传时，在上传文件的同时还需要上传参数，报错“ContextType:Multipart/form-dataxxxx” xxx not support

原因:使用了@RequestBody注解

