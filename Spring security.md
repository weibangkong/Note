***Spring Security***

1.UsernamePasswordAuthenciationFilter 继承之Abstract AuthenciationPorcessingFilter

负责调用加载用户信息，并调用身份认证调用，

认证通过后调用父类的successfulAuthenciation方法,方法内部调用remember me实现与 登录事件触发器

