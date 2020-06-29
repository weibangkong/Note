# **CoreJava**

#### 1.异常:

如果被调用这抛出了异常，调用者只是传递出去，而不是处理掉它,那么请不要try{}catch{}

##### 错误示例如下:

```java
public class UtilA {
	private static void doSomething() throw InterruptException{
		Thread.currentThread.sleep(5000);
	}
}


public class UtilB {
	private static void wait() throw InterruptException{
		System.out.println("wait");
		try{
			UtilA.doSomething();
		} catch(InterruptException e){
			throw e;
		}
	}
}

public Class C{
	public void stopWorld(){
		System.out.println("Ready go!!!")
		try{
			UtilB.wait();
		} catch(Exception e){
			log.error(...);
		}
		
	}
}
```

##### 正确示例:

```java
public class UtilA {
	private static void doSomething() throw InterruptException{
		Thread.currentThread.sleep(5000);
	}
}


public class UtilB {
	private static void wait() throw InterruptException{
		System.out.println("wait");
		UtilA.doSomething();
	}
}

public Class C{
	public void stopWorld(){
		System.out.println("Ready go!!!")
		try{
			UtilB.wait();
		} catch(Exception e){
			log.error(...);
		}
		
	}
}
```

#### 2.大文件上传计算md5摘要时OOM问题

##### 形成原因:

将文件的所有内容全部一次加载到内存中，然后对整个oom进摘要导致

##### 解决方法:

**1.增大JVM内存**(这是一个合格的程序员应该采用的办法么)

**2.每次对一部分内容进行摘要计算，然后汇总成为一个md5串**(推荐这样做，平时写代码使用IO操作时也应该注意)

