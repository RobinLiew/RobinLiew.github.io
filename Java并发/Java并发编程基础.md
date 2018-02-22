因为最近的项目遇到大量并发编程的问题，抽出时间把并发编程的基础整理一下，毕竟万丈高楼平地起。更深层次的并发编程会在以后的博客中介绍。
#一、为什么要并发编程？
- 并发编程可以使程序的执行速度更快。多处理器的情况并发编程可以更好的发挥多个处理器的能力，从而提高程序的执行速度，但是，并发编程通常是提高运行在单处理器上的程序性能。初看，线程间的上下文切换会增大开销，并不能提高程序的执行性能。但是，**阻塞**使情况变得不同。如果程序中的某个任务因为该程序控制范围之外的某些条件（通常是I/O）而导致不能继续执行，此时该线程阻塞，并发使得其他的线程可以继续处理其他的任务，而不用一直等待，从而提高了程序整体的执行效率。
- 实现并发编程最直接的方式是在操作系统级别使用**进程**。进程是运行在他自己的地址空间内的自包容程序。像Java所使用的这种并发系统会共享诸如内存和I/O这样的资源，因此编写多线程程序最基本的困难在于协调不同线程驱动的任务之间对这些资源的使用，以使得这些资源不会同时被多个任务访问。

#二、Java并发编程基础知识点
## 定义任务
- 线程可以驱动任务，因此需要一种描述任务的方式，这可以由Runnable接口来提供。任务的run()方法通常总会有某种形式的循环，使得任务一直运行下去直到不再需要，所有要设定跳出循环的条件condition。
```
	public class TaskDemo impement Runnable{
					public void run(){
						while(condition){
							//doSomething
						}
				}
	} 
```
	


##  Thread.yield()方法
- 在run()中对静态方法Thread.yield()的调用是对线程调度器的一种建议。它声明：“我已经执行完生命周期中最重要的部分了。此刻正是切换给其他任务执行一段时间的大好时机”。当调用yield()方法时，你也是在建议具有相同优先级的其他线程可以运行。
## Thread类
- 将Runnable对象转变为工作任务的传统方式是把它提交给一个Thread构造器。

```
	Thread t=new Thread(new TaskDemo());
	t.start();
```


 
## 使用Excecutor
- Java SE5的java.util.concurrent包中的执行器（Executor）将为你管理Thread对象，从而简化了并发编程。Executor在客户端和任务之间提供了一个间接层；与客户端直接执行任务不同，这个中介对象将执行任务。Executor容许你管理异步任务的执行，而无需显示的管理线程的生命周期，也就是说不用显示的调用开启线程的start（）方法。非常常见的情况是，单个的Executor被用来创建和管理系统中的所有任务。
-  对shutdown()方法的调用可以防止新的任务被提交给这个Executor对象，当前线程将继续运行在shutdown（）被调用之前所提交的的所有任务。这个程序将在Executor中的所有任务完成之后尽快推出。
		

	```
	这里写代码片ExecutorService exec=Executors.newCachedThreadPool();
	for(int i=0;i<5;i++){
		exec.execute(new TaskDemo());
	}
	exec.shutdown();
	```
- 在介绍下面几个Executors的方法前，先介绍一下几个类，接口，方法之间的关系

	```
	<<interface>>Executor <-- <<interface>>ExecutorService <-- ThreadPoolEcecutor
	<-- 返回 Executors(class) 静态方法：1,newCacheThreadPool();2,newFixedThreadPool(number);3,newSingleThreadExecutor();
	```
- 有了FixedThreadPool就可以一次性预先执行代价高昂的线程分配，因而也就可以限定线程的数量了。这可以节省时间，因为不用为每个人物都固定的付出创建线程的开销。当然CachedThreadPool在执行程序的过程中通常会创建与所需数量相同的线程，然后在它回收旧线程时停止创建新线程，因此它是合理的Executor的首选。
- SingleThreadPool就像是线程数量为1的FixedThreadPool。这对于你希望在另一个线程中连续运行的任何事物（长期存活的任务）来说是很有用的，例如监听进入的套接字连接的任务。如果向SingleThreadExecutor提交了多个任务，那么这些任务将排队，每个任务都会在下一个任务开始之前运行结束，所有的任务将使用相同的线程。SingleThreadExecutor会序列化所有提交给它的任务，并维护它自己（隐藏）的悬挂任务队列。
## 从任务中产生返回值

- Runnable是执行工作的独立任务，但是它不返回任何值。如果你希望任务在完成时能够返回一个值，那么可以实现Callable接口而不是Runnable接口。在Java SE5中引入的Callable是一种具有类型参数的泛型，它的类型参数表示的是从方法call()（而不是run()）中返回的值，并且必须使用ExecutorService.submit()方法调用它。
	```
	class TaskWithResult implements Callable<String>{
		public String call(){
			return "xxx";
		}
	}
	
	ArrayList<Future<String>> results=new ArrayList<Fureture<String>>();
	for(int i=0;i<10;i++){
		results.add(exec.submit(new TaskWithResult()));
	}
	```
- submit()方法会产生Future对象，它用Callable返回结果的特定类型进行了参数化。你可以用isDone()方法来查询Future是否已经完成。当任务完成时，它具有一个结果，你可以调用get()方法来获取该结果。你也可以不用isDone进行检查就直接调用get(),在这种情况下，get()将阻塞，直至结果准备就绪。

##   休眠
- 休眠任务行为的一种简单方法是调用sleep()，这将使任务终止执行给定的时间。任务睡眠（即阻塞），这使得线程调度器可以切换到另一个线程，进而驱动另一个任务。但是顺序行为依赖于底层的线程机制，这种机制在不同的操作系统之间是有差异的，因此，你不能依赖于它。
##  优先级
- 线程的优先级将该线程的重要性传递给了调度器。尽管CPU处理现有线程集的顺序是不确定的，但是调度器将倾向于让优先权最高的线程先执行。然而，这并不意味着优先权较低的线程将得不到执行。优先权较低的线程仅仅是执行的频率较低。JDK有10个优先级，一般只使用Thread.MAX_PRIORITY，Thread.NORM_PRIORITY,Thread.MIN__PRIORITY。

	```
	getPriority(...);//读取现有线程的优先级
	setPriority();//修改现有线程的优先级
	Thread.currentThread().setPriority(...);
	
	```
##   后台线程
  - 所谓后台（Daemon）线程，是指在程序运行的时候在后台提供一种通用服务的线程，并且这种线程并不属于程序中不可或缺的部分。因此，当所有的非后台线程结束时，程序也就终止了，同时会杀死进程中的所有后台线程。反过来说，只要有任何非后台线程还在运行，程序就不会终止（例如执行main()的就是一个非后台线程）。
  

	```
	public SimpleDaemons implement Runnable{
		public void run(){
			//dosomething();
		}
		public static void main(String[] args) throws Exception{
			Thread daemon=new Thread(new SimpleDaemons());
			daemon.setDaemon(true);
			daemon.start();
		}
	}
	```
 - 必须在线程启动之前调用setDaemon()方法，才能把它设置为后台线程。
 - 通过编写定制的ThreadFactory可以定制由Executor创建的线程的属性（后台，优先级，名称）

	```
	public class DaemonThreadFactory implement ThreadFactory{
		public Thread newThread(Runnable r){
			//dosomething;//设置线程的属性操作
		}
	}
	ExeccutorService exec=Executors.newCacheThreadPool(new DaemonThreadFactory());
	exec.execute(new DaemonFromFactory);//DaemonFromFactory类是一个实现了Runnable接口的类，在DaemonFromFactory类中覆写run方法
	```
- 可以通过调用isDaemon()方法来确定线程是否是一个后台线程。如果是一个后台线程，那么它创建的任何线程将被自动设置为后台线程。

##  加入一个线程

- 一个线程可以在其他线程之上调用join()方法，其效果是等待一段时间直到第二个线程才能继续执行。如果某个线程在另一个线程t上调用t.join()，此线程将被挂起，直到目标线程t结束才恢复。
	

	```
	线程A             |          线程B
	B.join()          | 
	A线程挂起，B运行完  |
	A才能继续运行      |
	```
- 也可以在调用join()时带上一个超时参数（单位可以是毫秒，或者毫秒和纳秒），这样如果目标线程在这段时间到期时还没有结束的话，join()方法总能返回。
- 对join()方法的调用可以被中断，做法是在调用线程（即调用join()方法的线程）上调用interrupt()方法，这时需要用到try-catch语句。
	
##  捕获异常
- 由于线程的本质特征，使得你不能捕获从线程中逃逸的异常。一旦异常逃出任务的run()方法，它就会向外传播到控制台，除非你采取特殊的步骤捕获这种错误的异常。Java SE5中，可以用Executor来解决这个问题。
	

	```
	//此类中设定对应线程中抛出异常的处理
	class MyUnCaughtExceptionHandler implements Thread.UnCaughtExceptionHandler {
		public void uncaughtException(Thread t,Throwable e){
			...
		}
	}
	//该线程工厂类中设定线程异常的监听器
	class Handler implements ThreadFactory{
		public Thread newThread(Runnable r){
			Thread t=new Thread(r);
			t.setUnCaughtExceptionHandler(new MyUnCaughtExceptionHandler());
		}
	}
	//使用定制化的线程工厂类创建线程池，并开启线程
	ExcutorService exec =Executors.newCachedThreadPool(new HandlerThreadFactory());
	exec.execute(new ExceptionThread());
	```
##  解决共享资源竞争
- 基本上所有的并发模式在解决线程冲突的时候，都采用序列化访问共享资源的方案。这意味着在给定时刻只容许一个任务访问共享资源。通常这是在代码前面加上一条锁机制语句来实现的，这就使得在一段时间内只有一个任务可以运行这段代码。因为锁语句产生了一种互相排斥的效果，所以这种机制常常称为互斥量（mutex）。
- 共享资源一般是以对象形式存在的内存片段，但也可以是文件、输入/输出接口，或者是打印机。要控制对共享资源的访问，得先把他包装进一个对象。然后把所有要访问这个资源的方法标记为synchronized。
- 注意，在使用并发时，将域设置为private是非常重要的，否则，synchronized关键字就不能防止其他任务直接访问域，这样就会产生冲突。
- 当同步方法访问操作共享资源时，其他的普通方法可以访问共享资源。因此，如果在你的类中有超过一个方法在处理临界数据，那么你必须同步所有相关的方法。如果只同步一个方法，那么其他方法将会随意地忽略这个对象锁，并可以在无任何惩罚的情况下被调用。这是很重要的一点：每个访问临界共享资源的方法都必须被同步，否则它们就不会正确的工作。
	 
##  使用显式的Lock对象
- Lock对象必须被显式的创建、锁定和释放。因此，它与内建的锁形式相比，代码缺乏优雅性。但是，对于解决某些类型的问题来说，它更加灵活。
	 

	```
	//Lock使用例子
	private Lock lock=new ReentrantLock();
	lock.lock();
	try{
		...
	}finally{
		lock.unlock();
	}
	```
- 当你使用Lock对象时，将这里所示的惯用法内部化是很重要的：紧接着的对lock()的调用，你必须放置在finally子句中带有unlock()的try-finally语句中。return语句必须在try子句中出现，以确保unlock()不会过早的发生，从而将数据暴露给第二个任务。
- 大体上，当你使用synchronized关键字时，需要写的代码量更少，并且用户错误出现的可能性也会降低，因此通常只有在解决特殊问题时，才使用显示的Lock对象。例如，用synchronized关键字不能尝试获得锁并且最终获取锁失败，或者尝试着获取锁一段时间，然后放弃它。
	

	```
	//尝试获取锁
	boolean captured=false;
	captured=lock.tryLock();
	try{
		...
	}finally{
		lock.unLock();
	}

	//尝试获取锁一段时间
	try{
		captured=lock.tryLock(2,TimeUnit.SECONDS);
	}catch(InterruptedException e){
		throw new RuntimeException(e);
	}
	try{
		...
	}finally{
		lock.unLock();
	}
	
	```

##  volatile关键字
- 介绍volatile关键字前，先简单介绍一下Java内存模型一些性质。
- Java内存模型规定：
	- 所有的变量都是存在主存中（相当于物理内存），
	- 每个线程都有自己的工作内存（高速缓存），
	- 线程对变量的操作都必须在工作内存中进行，而不能直接对主存进行操作，
	- 每个线程不能访问其他线程的工作内存
	- 例：i=10; 执行线程必须现在自己的工作线程中对变量i所在的缓存进行赋值操作，然后再写入主存当中，而不是直接将数值10写入主存中。
	 
- Java对原子性、可见性，以及有序性提供的保证：
	- 原子性：在Java中，对基本数据类型的变量的读取操作是原子性操作，即这些操作是不可中断的，要么执行，要么不执行。
			

		```
		//只有第一个语句是原子性操作
		x=10;
		y=x;
		x++;
		x=x+1;
		```
	- 可见性：对于可见性，Java提供了volatile关键字来保证可见性。当一个共享变量被volatile修饰时，它会保证修改的值立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值。
	- 有序性：在Java内存模型中，允许编译器和处理器对指令进行重新排序，但重排序过程不会影响到单线程程序的执行，却会影响到多线程并发执行的正确性。在Java中，可以通过volatile保证一定的有序性。
- 深入剖析volatile关键字
	- 一旦一个共享变量（类的成员变量，类的静态成员变量）被volatile修饰，则具备两层含义。
		- 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这个新值对其他线程来说是立即可见的。
		- 禁止进行指令重排
	- volatile关键字无法保证对变量的所有操作都是原子性的。
	- volatile关键字禁止指令重排的两层意思：
		- 当程序执行到volatile变量的读取操作或写操作时，在其前面的操作的更改肯定全部已经进行，且结果已经对后面的操作可见，在其后面的操作肯定还没有进行。
		- 在进行指令优化时，不能将在对volatile变量访问的语句放在其后面进行，也不能把volatile变量后的语句放到其前面执行。
	- volatile关键字的原理和实现机制
		- “观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令”。lock前缀指令实际上相当于一个内存屏障（也成内存栅栏）。
	- 使用volatile关键字的场景
		- synchronized关键字防止多个线程同时执行一段代码，那么就会很影响执行效率，而volatile关键字在某些情况下性能要由于synchronized，但要注意volatile关键字无法替代synchronized关键字，因为volatile关键字无法保证操作的原子性。
	- 使用volatile必须具备的两个条件：
		- 对变量的操作不依赖于当前值
		- 该变量没有包含在其他变量的不变式中
		- 使用volatile最常见的场景
			

		```
		//volatile修饰状态标记量
		volatile boolean init=false;
		//==线程1=====================
		context=loadContext();
		inited=true;
		//===========================
		
		//==线程2====================
		while(!inited){
			sleep();
		} 
		doSomethingWithConfig(context);
		```
##  原子类
- Java SE5引入了诸如AtomicInteger、AtomicLong、AtomicReference等特殊的原子性变量类。这些类被调整为可以使用在某些现代处理器上可获得的，并且是在机器级别上的原子性，因此使用它们时，通常不需要担心。对于常规编程来说，它们很少会排上用场，但是在涉及性能调优时，它们就大有用武之地了。

##  临界区
- 有时，你只是希望防止多个线程同时访问方法内部的部分代码而不是防止访问整个方法。通过这种方式分离出来的代码段被称为临界区，它也可以使用synchronized关键字建立。这里，synchronized被用来指定某个对象，此对象的锁被用来对花括号内的代码进行同步控制.这也被称为同步控制块；在进入此代码段前，必须得到syncObject对象的锁。如果其他线程已经得到这个锁，那么就得等到锁被释放以后，才能进入临界区。
	 

	```
	synchronized(syncObject){
		...
	}
	```
- synchronized块必须给定一个在其上进行同步的对象，并且最合理的方式是，使用其方法正在被调用的当前对象：synchronized(this)。
	
##  线程本地存储
 - 防止任务在共享资源上产生冲突的第二种方式是根除对变量的共享。线程本地存储是一种自动化机制，可以为使用相同变量的每个不同线程都创建不同的存储。
	 

	```
	private static ThreadLocal<T> connThreadLocal = new ThreadLocal<T>();
	```
 - ThreadLocal 对象通常当做静态域存储。在创建ThreadLocal时，你只能通过get()和set()方法来访问该对象的内容，其中get()方法将返回与线程相关联的对象的副本，而set()会将参数插入到其线程存储的对象中，并返回存储中原有的对象。 
	
## 线程的状态
 - 新建（new）：当线程被新建时，它只会短暂地处于这种状态。此时它已经分配了必须的系统资源，并执行了初始化。此刻线程已经有资格获得CPU时间了，之后调度器将把这个线程转变为可运行状态。
      
 - 就绪（Runnable）：在这种状态下，只要调度器把时间片分配给线程，线程就可以运行。也就是说，在任意时刻，线程可以运行也可以不运行。只要调度器能分配时间片给线程，它就可以运行；这不同于死亡和阻塞状态。
 - 阻塞（Blocked）：线程能够运行，但有某个条件阻止它的运行。当线程处于阻塞状态时，调度器将忽略线程，不会分配给线程任何CPU时间。直到线程重新进入了就绪状态，它才有可能执行操作。
 - 死亡（Dead）：处于死亡或终止状态的线程将不再是可调度的，并且再也不会得到CPU时间，它的任务已经结束，或不再是可运行的。任务死亡的通常方式是从run()方法返回，但是任务的线程还可以被中断。
      
##  进入阻塞状态
- 通过调用sleep(milliseconds)使任务进入休眠状态，在这种情况下，任务在指定的时间内不会运行。
- 你通过wait()使线程挂起。直到线程得到了notify()或notifyAll()消息，线程才会进入就绪状态。
- 任务在等待某个输入/输出完成
- 任务试图在某个对象上调用其同步控制方法，但对象锁不可用，因为另一个任务已经获取了这个锁。 

## 线程的中断
- Thread.interrupt()方法不能中断一个正在运行的线程，这一方法实际完成的是，在线程受到阻塞时抛出一个中断信号，这样就可以退出阻塞状态。更确切的说，如果线程被object.wait(),Thread.join(),Thread.sleep()三种方法之一阻塞，那么，它将收到一个中断异常（InterruptedException），从而提早地终结被阻塞的状态。
	
- 中断线程最推荐的方式是使用共享变量（shared variable）发出信号，告诉线程必须停止正在运行的任务。
- 如果是被object.wait(),Thread.join(),Thread.sleep()三种方法之一阻塞，正常终止线程的方式是设置共享变量，并调用interrupt()（注意，变量应先设置）。
- 中断I/O操作
	   - 如果正在使用通道channels（Java1.4引入的新的I/O API），那么被阻塞的线程将收到一个ClosedByInterruptException异常。
	   - 如果使用的是Java1.0之前就存在的传统I/O，Thread.interrupt()将不起作用，因为线程将不会退出被阻塞状态。
	   - Java平台为这种情形提供了一种解决方案，即调用阻塞该线程的套接字close方法。这种情形下，如果线程I/O操作阻塞，该线程将接收到一个SocketException异常，这与使用interrupt()方法引起一个InterruptedException异常被抛出非常相似。
	   - 与I/O调用不同，interrupt()可以打断被互斥所阻塞的调用。
	   

##  关闭线程池的方法
- 前面讲到Excutors类关闭线程池的方法，这里详细的介绍一下。
- shutdown()方法：当线程池被调用该方法时线程池的状态立刻变成SHUTDOWN状态。此时，则不能再往线程池中添加任何任务，否则将会抛出RejectedExecutionException异常。但是，此时线程池不会立刻退出，直到添加到线程池中的任务已经处理完毕，才能退出。
- shutDownNow()方法：执行该方法线程池的状态立刻变成STOP状态，并试图停止所有正在执行的线程，不再处理还在池队列中等待的任务，当然，它会返回那些未执行的任务。它会试图终止线程的方法是通过调用Thread.interrupt()方法来实现，但这种方法有限。例如，线程中没有sleep,wait,condition,定时锁等应用，interrupt方法是无法中断当前线程的。

- interrupt、interrupted、isInterrupt的区别
	- interrupt 设置线程的状态位为中断状态。
	- interrupted 作用于当前线程，返回线程的状态位后，要清掉原来的状态位（恢复为原来的样子）。
	

	```
	public static interrupted(){
		return currentThread.isInterrupted(true);
	}
	```
	- isInterrupted 作用于调用该方法的线程对象对应的线程 ，直接返回线程的状态位。
	

	```
	public native boolean isInterrupted(){
		return currentThread.isInterrupted(false);
	}
	```

#  三、线程之间的协作
 - 只能在同步控制方法或同步控制块里调用wait()、notify()、notifyAll()（因为不用操作锁，所有sleep()可以在非同步控制块里调用）。如果非同步控制方法里调用这些方法，程序能通过编译，但运行的时候，将得到IllegalMonitorStateException异常，并伴随着一些含糊的消息，比如“当前线程不是拥有者”。
## 使用notify，notifyAll,wait的原则
- 永远在synchronized的函数或对象里使用notify，notifyAll,wait，不然Java虚拟机会生成notify，notifyAll,wait异常。
- 永远在while循环里而不是if语句下使用wait。这样，循环会在线程睡眠前后检查wait条件，并在条件实际未改变的情况下使用wait。这样，循环会在线程睡眠前后检查wait条件，并在条件实际未改变的情况下处理唤醒通知。
	

	```
	while(condition){
		object.wait();
	}
	doSomething();
	//注意wait()方法写在while和if中的区别
	if(){
		object.wait();
	}
	doSomething();
	```
- 永远在多线程间共享的对象（在生产者消费者模型里即缓冲）上使用wait。
- 倾向于使用notifyAll()而不是notify()。当notifyAll()因某个特定锁而被调用时，只有等待这个锁的任务才会被唤醒。
- 使用显式的Lock和Condition对象。Condition：await();signalAll();   每个对Lock()的调用都必须紧跟一个try-finally语句，用来保证在所有情况下都可以释放锁。  
   

	```
	 private Lock lock=new ReentrantLock();
	 private Condition condition=lock.newCondition();
	 public coid method1() throws InterruptedException{
		 lock.lock();
		 try{
			while(...)
			condition.wait();
	         }finally{
		         lock.unLock();
	         }
	 }  
	 public coid method2() throws InterruptedException{
		 lock.lock();
		 try{
			while(...)
			condition.signalAll();
	         }finally{
		         lock.unLock();
	         }
	 }  
	```
##生产者-消费者与队列
- wait()和notifyAll()方法以一种非常低级的方式解决了任务互操作的问题，即每次交互时都握手。在许多情况下，使用更高级别的抽象，使用同步队列解决问题，同步队列在任何时刻都只容许一个任务插入或移除元素。
- 队列接口Queue
- BlockingQueue接口继承了Queue接口。
- 如果试图向一个已经满了的阻塞队列中添加一个元素或者是从一个空的阻塞队列中移除一个元素，将导致线程阻塞。在多线程合作时，阻塞队列是很有用的工具。
	

	```
	Queue:  offer()
		peek()
		element()
		poll()
		remove()
	BlockingQueue:  put()添加一个元素，如果队列满，则阻塞
			take()移除并返回队列头部的元素，如果队列为空，则阻塞
	```
- BlockingQueue接口的实现类：
	- ArrayBlockingQueue  (固定尺寸的)。a.基于数组的阻塞队列实现；b.在生产者放入数据和消费者获取数据，都是共用同一个锁对象。这也意味着两者无法真正并行运行。
	- LinkedBlockingQueue   （无界队列）。a.基于链表的阻塞队列；b.生产者和消费者分别采用了独立的锁来控制数据同步。
		
 ##任务间使用管道进行输入/输出
- 通过输入/输出线程间进行通信很有用。提供线程功能的类库以“管道”的形式对线程间的输入输出提供了支持。它们在Java输入/输出类库中的对应物就是PipedWriter类（允许任务向管道写）和PipedWriter类（允许不同任务从同一管道中读取）。

	```
	class Sender implements Runnable{
		private PipedWriter out=new PipedWriter();
		public PipedWriter getPipedWriter(){return out;}
		public void run(){
			while(true){
				...
				out.write(数据);
			}
		}
	}
	class Reveiver implements Runnable{
		private PipedReader in;
		public Receiver(Sender sender){
			in=new PipedReader(sender.getPipedWriter());
		}
		public void run(){
			while(true){
				...
				in.read();
			}
		}
	}
	public class PipedDemo{
		public static void main(String[] args){
			Sender sender=new Sender();
			Receiver receiver=new Receiver(sender);
			//开启两个线程
			...
		}
	}
	```
  ##死锁
  -   每个任务在等待另一个任务，而后者又等待别的任务，这样一直下去，直到这个链条上的任务又在等待第一个任务释放锁。这得到了一个任务之间相互等待的连续循环，没有哪个线程能继续。这被称为死锁。
  - 当一下四个条件同时满足时，就会发生死锁：
	  - 互斥条件。任务使用的资源中至少有一个是不能共享的。
	  - 至少有一个任务它必须持有一个资源且正在等待获取一个当前被别的任务持有的资源
	  - 资源不能被任务抢占，任务必须把资源释放当做普通事件
	  - 必须有循环等待，这时，一个任务等待其他任务所持有的资源，后者又在等待另一个任务所持有的资源，这样一直下去，直到有一个任务在等待第一个任务所持有的资源，使得大家都被锁住。
	