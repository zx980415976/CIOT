# Java Runnable与Callable
## 接口定义
### Callable接口
```
public interface Callable<V> {
    V call() throws Exception;
}
```
### Runnable接口
```
public interface Runnable {
    public abstract void run();
}
```
* 相同点 
1.都是接口   2.都可以编写多线程程序  3.都采用Thread.start()启动线程
* 不同点
1. Runnable没有返回值 ,Callable可以返回执行结果 ,和Future、FutureTask配合可以用来获取异步执行的结果。
2. Callable接口的call()方法允许抛出异常，Runnable的run()方法异常只能在内部消化，不能往上继续抛。
## Runnable的使用

* 第一种写法：

```
 public class MyThread extends Thread{
	public void run() {
		System.out.println("I am a runnable tread task");
	}
}
```
* 第二种写法：
```
public class MyThread2 implements Runnable{
@Override
public void run() {
	System.out.println("I am a runnable thread task");
	
}
}

Thread thread = new Thread(new MyThread2());
		thread.start();
```
* 第三种写法：
```
        ExecutorService executor = Executors.newCachedThreadPool();
        RunnableTask runnableTask = new RunnableTask();
        executor.execute(runnableTask);

```
* 使用线程池的好处
1. 减少对象创建、消亡的开销，性能佳。
2. 可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。
3. 提供功能多

## Callable的使用
```
public class IntegerCallableTask implements Callable<Integer> {

	 @Override
	    public Integer call() throws Exception {
			System.out.println("I am a callnable thread task");
	        int sum = 0;
	        for (int i = 0; i < 100; i++) {
	            sum += i;
	        }
	        return sum;
	    }
}

public class ThreadTest {
	
	public static void main(String[] args) {
		FutureTask<Integer> fu = new FutureTask<Integer>(new IntegerCallableTask());
		Thread thread = new Thread(fu);
		thread.start();
		try {
            System.out.println("返回值是:" + fu.get());
        } catch (Exception e) {
            e.printStackTrace();
        }
	}

}

//I am a callnable thread task
//返回值是:4950
```
* 修改成线程池方式
```
public class ThreadTest {

	public static void main(String[] args) {
		ExecutorService executor = Executors.newCachedThreadPool();
		IntegerCallableTask integerCallableTask = new IntegerCallableTask();
		Future<Integer> future = executor.submit(integerCallableTask);
		executor.shutdown();
		try {
			System.out.println(future.get());
		} catch (Exception e) {
			e.printStackTrace();
		}

	}
}
```
* Callable任务通过线程池的submit方法提交。且submit方法返回Future对象，通过Future的get方法可以获得具体的计算结果, get方法会阻塞当前线程，如果任务未执行完，则一直等待。

## 关于Future和FutureTask

* 对于Callable来说，Future和FutureTask均可以用来获取任务执行结果，Future是个接口，FutureTask是Future的具体实现，FutureTask间接实现了Runnable接口，也就是说FutureTask可以作为Runnable任务提交给线程池。

* Future模式理解：我有一个任务，提交给了Future，Future替我完成这个任务。期间我自己可以去做任何想做的事情。一段时间之后，我就便可以从Future那儿取出结果。
1. Future提供了三种功能：isDone 判断任务是否完成，isCancelled 能够中断任务，get 能够获取任务执行的结果。
2. 向线程池中提交任务的submit方法不是阻塞方法，而Future.get方法是一个阻塞方法，当submit提交多个任务时，所以只有所有任务都完成后，才能使用get按照任务的提交顺序得到返回结果，所以一般需要使用future.isDone先判断任务是否全部执行完成，完成后再使用future.get得到结果。




