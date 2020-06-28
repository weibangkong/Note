# **CoreJava**

### 1.异常:

如果被调用这抛出了异常，调用者只是传递出去，而不是处理掉它,那么请不要try{}catch{}

错误示例如下:

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

正确示例:

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

2.