##第十三章 多线程##

-	1，线程简介
	-	Java中的多线程在每个操作系统中的运行方式也存在差异，在此着重说明多线程在Windows操作系统中的运行模式。
	
	-	Windows操作系统是多任务操作系统，它以进程为单位。
	一个进程是一个包含有自身地址的程序，每个独立执行的程序都称为进程，也就是正在执行的程序。
	
	-	系统可以分配给每个进程一段有限的使用CPU的时间（也称为CPU时间片），CPU在这段时间中执行某个进程，然后下一个时间片又跳至另一个进程中去执行。
	
	-	由于CPU转换较快，所以使得每个进程好像是同时执行一样。
	
	-	一个线程则是进程中的执行流程，一个进程中可以同时包括多个线程，每个线程也可以得到一小段程序执行时间，这样一个进程就可以具有多个并发执行的线程。
	
	-	在单线程中，程序代码按调用顺序依次往下执行，如果需要一个进程同时完成多段代码的操作，就需要产生多线程。

-	2，线程的实现方式	
	
	-	在Java中主要提供两种方式实现线程：分别为继承java.lang.Thread类与实现java.lang.Runnable接口。
	
	-	继承Thread类：Thread类是java.lang包中的一个类，从这个类中实例化的对象代表线程，程序员启动一个新线程需要建立Thread实例。
		-	Thread类中常用的构造方法：
				
				public Thread(String threadName);创建一个名称为threadName的线程对象。
				public Thread();
		
			-	完成线程真正功能的代码放在类的Run()方法中，当一个类继承Thread类后，就可以在该类中覆盖run()方法，将实现该线程功能的代码写入run()方法中，
		
			-	然后同时调用Thread类中的start()方法执行线程，也就是调用run()方法。
		
			-	run()方法的格式必须是：public void run(){}
	
		-	当执行一个线程程序时，就自动产生一个线程，主方法正是在这个线程上运行的。当不再启动其他线程时，该程序就为单线程程序。
	
		-	主方法线程启动由Java虚拟机负责，程序员负责启动自己的线程。
	
		-	通常在run()方法中使用无限循环的形式，使得线程一直运行下去，所以要指定一个跳出循环的条件。
	
		-	在main方法中，使线程执行需要调用Thread类中的start()方法，start()方法调用被覆盖的run()方法，如果不调用start()方法，线程永远都不会启动，在主方法没有调用start()方法之前，Thread对象只是一个实例，而不是一个正真的线程。
	
	-	实现Runable接口：如果程序员需要继承其他类（非Thread类），而且还要使当前类实现多线程，那么可以通过Runable接口来实现。
			
		-	实质上Thread类实现了Runnable接口，其中的run()方法正是对Runnable接口中的run()方法的具体实现。
			
			-	Thread类中有以下两个构造方法：
					
					public Thread(Runnable r);
					public Thread(Runnable r,String name);
			使用以上构造方法就可以将Runnable实例与Thread实例相关联。
			
			-	使用Runnable接口启动新线程的步骤如下：
					
				-	（1）建立Runnable对象。
				-	（2）使用参数为Runnable对象的构造方法创建Thread实例。
				-	（3）调用start()方法启动线程。
			
			-	通过Runnable接口创建线程时程序员首先需要编写一个实现Runnable接口的类，然后实例化该类的对象，这样就建立Runnable接口的类；
			
			-	接下来使用相应的构造方法创建Thread实例；最后使用该实例调用Thread类中的Start()方法启动线程。

-	3，线程的生命周期
	
	-	线程具有生命周期，其中包含7中状态，分别为出生状态、就绪状态、运行状态、等待状态、休眠状态和死亡状态。
	
	-	出生状态就是线程被创建时处于的状态，在用户使用线程实例调用start()方法之前线程都处于出生状态；
	
	-	当用户调用start()方法后，线程处于就绪状态（又被称为可执行状态）；
	
	-	当线程得到系统资源后就进入运行状态；
	
	-	一旦线程进入可执行状态，它会在就绪与运行状态下转换，同时也有可能进入等待、休眠、阻塞状态或死亡状态；
	
	-	当处于运行状态下的线程调用Thread类中的wait()方法时，该线程便进入等待状态，进入等待状态的线程必须调用Thread类中的notify()方法才能被唤醒，而notifyAll()方法是将所有处于等待状态下的线程唤醒；
	
	-	当线程调用Thread类中的sleep()方法时，则会进入休眠状态。
	如果一个线程在运行状态下发出输入/输出请求，该线程将进入阻塞状态，在其等待输入/输出结束时线程进入就绪状态；
	
	-	对于阻塞的线程来说，即使系统资源空闲，线程依然不能回到运行状态；
	
	-	当线程的run()方法执行完毕时，线程进入死亡状态。
	
	-	使线程处于就绪状态有以下几种方法：
		
		-	调用sleep()方法；
		-	调用wait()方法；
		-	等待输入/输出完成；
	
	-	当线程处于就绪状态后，可以使用以下几种方法使线程再次进入运行状态：
		-	线程调用notify()方法；
		-	线程调用notifyAll()方法；
		-	线程调用interrupt()方法；？
		-	输入/输出结束。

-	4，操作线程的方法
	
	-	线程的休眠：一种能控制线程行为的方法是调用sleep()方法，sleep()方法需要一个参数用于指定该线程休眠的时间，该时间以毫秒为单位。
	
	-	线程的加入：如果当前某程序为多线程程序，假如存在一个线程A，现在需要插入线程B，并且要求线程B先执行完毕，然后再继续执行线程A，此时可以使用Thread类中的join()方法来完成。
	
	-	线程的中断：以前使用stop()方法停止线程，现已废除。现在提倡在run()方法中使用无限循环的形式，然后使用一个布尔型标记控制循环的停止。
	
	-	线程的优先级：每个线程都有自己的优先级，线程的优先级可以表明在程序中该线程的重要性，
				  
		-	如果有很多线程处于就绪状态，系统会根据优先级来决定首先使哪个线程进入运行状态。但这并不意味着低优先级的线程得不到运行，而只是它们运行的几率比较小，如垃圾回收线程的优先级就较低。
				  
		-	Thread类中包含的成员变量代表了线程的某些优先级，如Thread.MIN_PRIORITY（常数1）、Thread.MAX_PRIORITY（常数10）、Thread.NORM_PRIORITY（常数5）。
				  
		-	在默认情况下其优先级都是5，每个新产生的线程都继承了父线程的优先级。setPriority()方法调整优先级。

-	5，线程的同步
	
	-	Java提供了线程同步机制来防止资源访问冲突。
	
	-	线程的同步机制：基本上所有解决多线程资源冲突问题的方法都是采用给定时间只允许一个线程访问共享资源，这是就需要给共享资源上一道锁。
					
		-	同步机制使用synchronized关键字。其语法如下：
				
				synchronized(Object){}
					
			-	通常将共享资源的操作放置在synchronized定义的区域内，这样当其他线程也获取到这个锁时，必须等待锁被释放时才能进入该区域。
				    
			-	Object为任意一个对象，每个对象都存在一个标志位，并且有两个值，分别为0和1。一个线程运行到同步块时首先检查该对象的标志位，如果为0状态，表明此同步块中存在其他线程在运行。这时线程处于就绪状态，直到处于同步块中的线程执行完毕为止。这时该对象的标志位被设置为1，该线程才能执行同步块中的代码，并将Object对象的标志位设置为0，防止其他线程执行同步块中的代码。
					
		-	同步方法：
							
				同步方法就是在方法前面修饰synchronized关键字的方法，语法：synchronized void f(){}
							
##第十四章 I/O##

-	1，输入/输出流
	
	-	Java语言定义了许多类专门负责各种方式的输入/输出，这些类都被放在java.io包中。
	
	-	其中，所有输入流都是抽象类InputStream（字节输入流）或抽象类Reader（字符输入流）的子类；
	
	-	而所有输出流都是抽象类OutputStream（字节输出流）或抽象类Writer（字符输出流）的子类。
	
	-	InputStream类常用的方法：read();从输入流中读取数据的下一个字节。返回0~255范围内的int字节值。如果因为已经到达流末尾而没有可用的字节，则返回-1。
							
		-	 read(byte[] b);从输入流中读入一定长度的字节，并以整数的形式返回字节数。
							 
		-	close();关闭此输入流并释放与该流关联的所有系统资源。
	
	-	字符流的由来：字节流读取文字字节数据后，不直接操作而是先查指定的编码表。获取对应的文字，再对这个文字进行操作。简单的说：字节流+编码表
	
	-	Java中的字符是Unicode编码，是双字节的。InputStream是用来处理字节的，并不适合处理字符文本。
	
	-	Reader类中的方法与InputStream类中的方法类似，读者在需要时可查看JDK文档。
	
	-	输出流：OutStream类是字节输出流的抽象类，此抽象类是表示输出字节流的所有类的超类。
			
		-	OutStream类中所有的方法返回void。
			
		-	常用的方法：
		
			-	write(int b);将指定的字节写入此输出流。
						
			-	write(byte[] b);将b个字节从指定的byte数组写入此输出流。
						
			-	write(byte[] b,int off,int len);将指定byte数组中从偏移量off开始的len个字节写入此输出流。
						
			-	flush();彻底完成输出并清空缓存区
						close();关闭输出流。
			
			-	Writer类是字符输出流的抽象类，所有字符输出类的实现都是它的子类。

-	2，File类
	
	-	File类是java.io包中唯一代表磁盘文件本身的对象。File类定义了一些与平台无关的方法来操作文件，可以通过调用File类中的方法，实现创建、删除、重命名文件等操作。
	
	-	File类的对象主要用来获取文件本身的一些信息，如文件所在的目录、文件的长度、文件读写权限等。数据流可以将数据写入到文件中，文件也是数据流最常用的数据媒体。
	
	-	文件的创建与删除：可以使用File类创建一个文件对象。通常使用以下3种构造方法来创建文件对象。
						
		-	File(String pathname);//该构造方法通过将给定路径名字符串转换为抽象路径名来创建一个新File实例。pathname指路径名称（包含文件名）
						
		-	File(String parent,String child);//该构造方法根据定义的父路径和子路径字符串（包含文件名）创建一个新的File对象。parent:父路径字符串。child:子路径字符串
						
		-	File(File f,String child);//该构造方法根据parent抽象路径名和child路径名称字符串创建一个新File实例。
				f:父路径对象
				child:子路径字符串 
	
	-	File类提供了很多方法用于获取一些文件本身信息。

-	3，文件输入/输出流
	
	-	程序运行期间，大部分数据都在内存中进行操作，当程序结束或关闭时，这些数据将消失。如果要将数据永久保存，可使用文件输入/输出流与指定的文件建立连接，将需要的数据永久保存到文件中。
	
	-	FileInputStream类（继承自InputStream类）与FileOutputStream类（继承自OutputStream类）都用来操作磁盘文件。
		-	FileInputStream类常用的构造方法：FileInputStream(String name);使用给定的文件名name创建一个FileInputStream对象
									
		-	FileInputStream(File file);使用File对象创建FileInputStream对象。
	
		-	FileOutputStream类有与FileInputStream相同的参数构造方法，创建一个FileOutputStream对象时，可以指定不存在的文件名，但此文件不能是一个已被其他程序打开的文件。
	
	-	FileReader和FileWriter字符流对应了FileInputStream和FileOutputStream类。
	
	-	FileReader流顺序地读取文件，只要不关闭流，每次调用read()方法就顺序地读取源中其余内容，直到源的末尾或流被关闭。
	
-	4，带缓存的输入/输出,
	
	-	缓存是I/O的一种性能优化。缓存流为I/O流增加了内存缓存区。
	
	-	BufferedInputStream类可以对所有InputStream类进行带缓存区的包装以达到性能的优化。
	
	-	两个构造方法：BufferedInputStream(InputStream in);该构造方法创建了一个带有32个字节的缓存流；
				  
	-	BufferedInputStream(InputStream in,int size);该构造方法按指定的大小来创建缓存区。
	
	-	BufferedOutputStream类有一个flush()方法用来将缓存区的数据强制输出完。
	
	-	两个构造方法：BufferedOutputStream(OutputStream in);
				  BufferedOutputStream(OutputStream in,int size);
				  
	-	BufferedReader类与BufferedWriter类分别继承Reader类与Writer类。这两个类同样具有内部缓存机制，并可以以行为单位进行输入/输出。
		-	BufferedReader常用的方法：
			
			-	read();读取单个字符
			-	readLine();读取一个文本行，并将其返回为字符串。若无数据可读，则返回null。
	
		-	BufferedWritr类中的方法都返回void。
	
			常用方法：
			
			-	write(String s,int off,int len);写入子符串的某一部分。
			-	flush();刷新该流的缓存。
			-	newLine();写入一个行分隔符。
	
	-	在使用BufferedWriter类的write()方法时，数据并没有立刻被写入至输出流，而是首先进入缓存区中。如果想立刻将缓存区中的数据写入输出流，一定要调用flush()方法。

-	5，数据输入/输出流
	
	-	数据输入/输出流（DataInputStream类与DataOutputStream类）允许应用程序以与机器无关的方式从底层输入流中读取基本Java数据类型。
	
	-	DataInputStream类和DataOutputStream类的构造方法：
				DataInputStream(InputStream in);
				DataOutStream(OutputStream out);
	
	-	DataOutputStream类提供了三种写入字符串的方法：
				writeBytes(String s);
				writeChars(String s);
				writeUTF(String s);
-	DataOutputStream类只提供了一个readUTF()方法返回字符串