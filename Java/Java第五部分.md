##第十五章 网络通信 

-	1，网络协议
	-	OSI模型
	-	IP协议：是Internet Protocol的简称，它是一种网络协议。
	-	Internet网络采用的协议是TCP/IP协议，其全称是Transmission Control Protocal/Internet Protocol。
	
		-	到目前为止IP地址用4个字节，也就是32位的二进制数来表示，称为IPv4。为了便于使用，通常取用每个字节的十进制数，并且每个字节之间用圆点隔开来表示IP地址。
	
	-	TCP/IP模式是一种层次结构，共分4层，分别为应用层、传输层、互联网层和网络层。各层实现特定的功能，提供特定的服务和访问接口，并具有相对的独立性。
	
	-	TCP与UDP协议：传输控制协议（transmission Control Protocol,TCP）与用户数据报协议（User Datagram Protocol,UDP）
				  
		-	TCP协议是一种以固结连线为基础的协议，它提供两台计算机间可靠的数据传输。TCP可以保证从一端数据送至连接的另一端时，数据能够确实送达，而且抵达的数据的排列顺序和送出时的顺序相同，因此，TCP协议适合可靠性要求比较高的场合。就像打电话。
				  
		-	UDP是无连接通信协议，不保证可靠数据的传输，但能够像若干个目标发送数据，接收发自若干个源的数据。UDP是以独立发送数据包的方式进行。
				 
			-	 就像邮递员送信。UDP协议适合于一些对数据准确性要求不高的场合，如网络聊天室、在线影片等。
 			-	这是由于TCP协议在认证上存在额外耗费，可能使传输速度减慢，而UDP协议可能会更适合这些对传输速度和实效要求非常高的网站，即使有一部分数据包遗失或传递顺序有所不同，也不会严重危害该项通信。
	
	-	端口和套接字：一般而言，一台计算机只有单一的连接到网络的物理连接（physics Connection），所有的数据都通过此连接对内、对外送达特定的计算机，这就是端口。
				  
		-	网络程序设计中的端口（port）并非真实的物理存在，而是一个假想的连接装置。
				  
		-	端口被规定为一个在0~65535之间的整数。
				  
			-	Http一般使用80端口；FTP使用21端口。通常，0~1023之间的端口数用于一些知名的网络和应用，用户的普通网络应用程序应该使用1024以上的端口数，以避免端口号与另一个应用或系统服务所用端口冲突。
				  
			-	网络中的套接字（Socket）用于将应用程序连接起来。套接字是一个假想的连接装置，就像插插头的设备“插座”用于连接电器与电线一样。
	
	-	InetAddress类：java.net包中的InetAddress类是与IP地址相关的类，利用该类可以获取IP地址、主机地址等信息。
				   
		-	常用的方法：
								
			-	getHostName();返回值为String，获取此IP地址的主机名
								
			-	getHostAddress();返回值为String，获取InetAddress对象所含的IP地址
								
			-	getLocalHost();返回值为InetAddress，返回本地主机的InetAddress对象
								
			-	getByName(String host);返回值为InetAddress，获取与Host相对应的InetAddress对象

-	2，UDP程序设计基础
	
	-	基于UDP通信的基本模式如下：
			
			将数据打包（称为数据包），然后将数据包发往目的地；
			接受别人发来的数据包，然后查看数据包。
	
	-	UDP程序的步骤：
			
		-	发送数据包：
			-	（1）使用DatagramSocket()创建一个数据包套接字；
			-	（2）使用DatagramPacket(byte[] buf,int offset,int length,InetAddress address,int port)创建要发送的数据包；
			-	（3）使用DatagramSocket类的send()方法发送数据包。
			
		-	接受数据包：
			-	（1）使用DatagramSocket(int port)创建数据包套接字，绑定到指定的端口；
			-	（2）使用DatagramSocket(byte[] buf,int length)创建字节数组来接收数据包；
			-	（3）使用DatagramSocket类的receive()方法接受UDP包。
			
	-	DatagramSocket类的receive()方法接收数据时，如果还没有可以接受的数据，在正常情况下receive()方法将阻塞，一直等到网络上有数据传来，receive()接受该数据并返回。
	
	-	DatagramPacket类：java.net的DatagramPacket类用来表示数据包。它的构造函数：
					  
		-	DatagramPacket(byte[] buf,int length);指定了数据包的内存空间和大小
					  
		-	DatagramPacket(byte[] buf,int length,InetAddress address,int port);不仅指定了数据包的内存空间和大小，还指定了数据包的目标地址和端口。
					
			-	在发送数据时，必须指定接收方的Socket地址和端口号，因此使用第二种构造函数可创建发送数据的DatagramPacket对象。
	
	-	DatagramSocket类：java.net包中的DatagramSocket类用于表示发送和接收数据包的套接字。该类的构造函数有：
					  
		-	DatagramSocket();创建DatagramSocket对象，构造数据包套接字并将其绑定到本租金上任何可用的端口。
					  
		-	DatagramSocket(int port);创建DatagramSocket对象，创建数据包套接字并将其绑定到本主机上的指定端口。
					  
		-	DatagramSocket(int port,InetAddress addr);创建DatagramSocket对象，创建数据包套接字，并将其绑定到指定的本地地址。
					  
			-	第三种构造方法适用于有多块网卡和多个IP地址的情况。接收程序时，必须指定一个端口号，不要让系统随机产生，此时可以使用第二中构造函数。发送程序时，通常使用第一种构造函数，不指定端口号，这样系统就会为我们分配一个端口。

-	3，TCP程序设计基础
	
	-	ServerSocket类：java.net包中的ServerSocket类用于表示服务器套接字，其主要功能是等待来自网络上的“请求”，它可以通过指定端口来等待连接的套接字。
					
	-	服务器套接字一次可以与一个套接字连接。如果多台客户机同时提出连接请求，服务器套接字会将请求连接的客户机存入列队中，然后从中取出一个套接字，与服务器新建的套接字连接起来。若请求连接数大于最大容纳数，则多出的连接请求被拒绝。队列的默认大小是50。
					
	-	ServerSocket类的构造方法都抛出IOException异常，有以下几种方式：
						
		-	ServerSocket();创建非绑定服务器套接字。
						
		-	ServerSocket(int port);创建绑定到特定端口的服务器套接字。
						
		-	ServrSocket(int port,int backlog);利用指定的backlog创建服务器套接字并将其绑定到指定的本地端口号。
					
		-	ServerSocket(int port,int backlog,InetAddress bindAddress);使用指定的端口、侦听backlog和要绑定到的本地IP地址创建服务器。
					
		-	ServerSocket类常用方法：accept();返回值为Socket，等待客户机的连接。若连接，则创建一套接字。
											
		-	getInetAddress();返回值为InetAddress或int。
											
		-	close();返回值为void，关闭服务器套接字。
					
	-	调用ServerSocket类的accept()方法会返回一个和客户端Socket对象相连接的Socket对象，服务器端的Socket对象使用getOutputStream()方法获得的输出流将指向客户端Socket对象使用getInputStream()方法获得的那个输入流；
	
	-	同样，服务器端的Socket对象使用getInputStream()方法获得的输入流将指向客户端Socket对象使用getOutputStream()方法获得的那个输出流。也就是说，当服务器向输出流写入信息时，客户端通过相应的输入流就能读取，反之亦然。
	
	-	在网络编程中如果只要求客户机向服务器发送信息，不要求服务器向客户机发送消息，称为单向通信。
	
	-	当一台机器上安装了多个网络应用程序时，很可能指定的端口号已被占用。还可能遇到以前运行良好的网络程序突然运行不了的情况，这种情况很可能也是由于
	
	-	端口号被别的程序占用了。此时可以使用netstat -an命令在DOS窗口中查看该程序所使用的端口。
	
##第十六章 反射
	
-	Class对象与Java反射
	
-	通过Java反射机制，可以在程序中访问已经装载到JVM中的Java对象的描述，实现访问、检测和修改描述Java对象本身信息的功能。
	
-	Java反射机制的功能十分强大，在java.lang.reflect包中提供了对该功能的支持。
	
-	Object类中定义了一个getClass()方法，该方法返回一个类型为Class的对象。
	
-	利用Class对象可以访问该对象的很多描述信息：
	
		例如，包路径、类名称、继承类、实现接口，构造方法、方法、成员变量，内部类、内部类的声明类等。
			
##第十七章 Swing程序设计

-	GUI（图形用户界面）为程序提供图形界面，它最初的设计目的是为了程序员构建一个通用的GUI，使其能够在所有平台上运行，但Java1.0 中基础类AWT（抽象窗口工具）并没有达到这个要求，于是Swing出现了，它是AWT组件的增强组件，但是它并不能完全替代AWT组件，这两种组件需要同时出现在一个图形用户界面中。
	
-	原来的AWT组件来自java.awt包，当含有AWT组件的Java应用程序在不同的平台上执行时，每个平台的GUI组件的显示会有不同，但是在不同平台上运行使用Swing
	
-	开发的应用程序时，，就可以统一GUI组件的显示风格，因为Swing组件允许编程人员在跨平台时指定统一的外观和风格。
	
-	Swing组件通常被称为“轻量级组件”，因为它完全由Java语言编写，而Java是不依赖于操作系统的语言，它可以在任何平台上运行，相反，依赖于本地平台的组件
	被称为“重量级组件”。
	

##第十八章 数据库操作

-	1，数据库基本知识
	
	-	数据库是一种存储结构，它允许使用各种格式输入、处理和检索数据，不必在每次需要数据的时重新输入。
	
	-	数据库的主要特点：实现数据共享，减少数据的冗余度，数据的独立性，数据实现集中控制，数据的一致性和可维护性（以确保数据的安全性和可靠性）。
	
	-	数据库的基本结构：物理数据层，概念数据层，逻辑数据层。
	
	-	数据库的种类：
		
		-	层次型数据库：层次型数据库类似于树结构，是一组通过链接而相互联系在一起的记录。层次模型的特点是记录之间的联系通过指针实现。由于层次模型层次顺序严格而且复杂，因此对数据进行各项操作都很困难。
		
		-	网状型数据库：网络模型是使用网络结构表示实体类型、实体间联系的数据模型。网络模型容易实现多对多的联系。但在编写应用程序时程序员需熟悉数据库的逻辑结构。
		
		-	面向对象型数据库：建立在面向对象模型的基础上。
		
		-	关系型数据库：关系型数据库是目前最流行的数据库，是基于关系模型建立的数据库，关系模型是由一系列表格组成。
	
	-	当前比较流行的数据库：MySQL数据库是开放源代码的软件，具有功能强、使用简便、管理方便、运行速度快、安全可靠性强等优点，同时也是具有客户机/服务器体系结构的分布式数据库管理系统。
						  
	-	其他数据库还有SQL Server,Oracle等高级数据库。
	从JDK6开始，在JDK的安装目录中除了传统的bin、jre等目录，还新增了一个名为db的目录，，这便是Java DB。这是一个纯Java实现的、开源的数据库管理系统（DBMS），源于Apache软件基金会（ASF）名下的项目Derby。

-	2，SQL语言
	-	SQL（Structure Query Language,结构化查询语言）被广泛地应用于大多数数据库中，使用SQL语言可以方便地查询、操作、定义和控制数据库中的数据。
	
	-	SQL语言主要由以下几部分组成：
			
		-	数据定义语言（Data Definition Language，DDL），如create，alter，drop等。
			
		-	数据操纵语言（Data Manipulation Language，DML），如select、insert、update、delete等。
			
		-	数据控制语言（Data Control Language，DCL），如grant、revoke等。
			
		-	事务控制语言（Transaction Control Language），如commit、rollback等。
	
	-	在应用程序中使用最多的就是数据操纵语言，它也是最常用的的核心SQL语言。
	
	-	几种基本的数据操纵语言：
	
		-	1.select语句：select语句用于从数据表中检索数据
		语法：select 所选字段列表 from 数据表名 where 条件表达式 group by 字段名 having 条件表达式（指定分组的条件） order by 字段名；
		
			例：select name,age from tb_emp where sex='女' order by age;//将tb_emp表中所有女员工的姓名、年龄按年龄升序的形式检索出来。
	
		-	2.insert语句：insert语句用于向表中插入新数据。
		语法：insert into 表名[(字段名1，字段名2...)] values(属性1，属性2，...)
		
			例：insert into tb_emp values(2,‘lili’,‘nv','销售部');//向数据表tb_emp（包含字段id,name,sex,department）中插入数据。
		
		-	3.update语句：update语句用于更新数据表中的某些记录。
		语法：update 数据表名 set 字段名=新的字段值 where 条件表达式 
		
			例：update tb_emp set age=24 where id=2;//将数据表tb_emp中2号员工的年龄修改为24。
	
		-	4.delete语句：delete语句用于删除数据。
		语法：delete from 数据表名 where 条件表达式
		
			例：delete from tb_emp where id=1024;//要删除数据表tb_emp中编号为1024的员工。

-	3，JDBC技术
	
	-	JDBC是一种可用于执行SQL语句的Java API（Application Programming Interface,应用程序设计接口），是连接数据库和Java应用程序的纽带。
	
	-	JDBC的全称是JavaDataBase Connectivity，是一套面向对象的应用程序接口，指定了统一的访问各种关系型数据库的标准接口。
	
	-	JDBC是一种底层的API，因此访问数据库时需要在业务逻辑层中嵌入SQL语句。SQL语句是面向关系的，依赖于关系模型，所以通过JDBC技术访问数据库也是面向关系的。
	
	-	JDBC技术完成的主要任务：
		
		-	与数据库建立一个连接；
		-	向数据库发送SQL语句；
		-	处理从数据库返回的结果。
	
	-	JDBC并不能直接访问数据库，必须依赖于数据库厂商提供的JDBC驱动程序。
	
	-	JDBC驱动程序的类型：JDBC的总体由4个组件-应用程序、驱动程序管理器、驱动程序和数据源组成。
	
	-	JDBC驱动基本上分为4种：
		
		-	JDBC-ODBC桥：依靠ODBC驱动器和数据库通信。这种连接方式必须将ODBC二进制代码加载到使用该驱动程序的每台客户机上。这种类型的驱动程序最适合于企业网或者用Java编写的三层结构的应用程序服务器代码。
		
		-	本地API一部分用Java编写的驱动程序：这类驱动器程序把客户机的API上的JDBC调用转换为Oracle、DB2、Sybase或者其他DBMS的调用。
		
		-	JDBC网络驱动：这种驱动程序将JDBC转换为与DBMS无关的网络协议，又被某个服务器转换为一种DBMS协议，是一种利用Java编写的JDBC驱动程序，也是最为灵活的JDBC驱动程序。
		
		-	本地协议驱动：这是一种纯Java的驱动程序。

-	4，JDBC中常用的类和接口
	
	-	常用的JDBC接口和类都在java.sql包中。
	
	-	Connection接口：Connection接口代表与特定的数据库的连接，在连接上下文中执行SQL语句并返回结果。
	
	-	Statement接口：Statement接口用于在已经建立连接的基础上向数据库发送SQL语句。在JDBC中有三种Statement对象，分别为Statement、PreparedStatement和CallableStatement。Statement对象用于执行不带参的简单的SQL语句；PreparedStatement继承了Statement，用来执行动态的SQL语句；
	CallableStatement继承了PreparedStatement，用于执行对数据库的存储过程的调用。
	
	-	DriverManager类：DriverManager类用来管理数据库中的所有驱动程序。它是JDBC的管理层，作用于用户和驱动程序之间，跟踪可用的驱动程序，并在数据库的驱动程序之间建立连接。如果通过getConnection()方法可以建立连接，则经连接返回，否则抛出SQLException异常。
	
	-	ResultSet接口：Result接口类似于一个临时列表，用来暂时存放数据库查询操作所获得的结果集。ResultSet实例具有指向当前数据行的指针，指针开始的位置在第一条记录的前面，通过next()方法可将指针向下移。


-	5，数据库的操作
	
	-	连接数据库：要访问数据库，首先要加载数据库的驱动程序（只需要在第一次访问数据库时加载一次），然后每次访问数据库时创建一个Connection对象，接着执行操作数据库的SQL语句，最后在完成数据库操作后销毁前面创建的Connection对象，释放与数据库的连接。
	
		-	通过java.lang包中的Class类的静态方法forName()来加载JDBC驱动程序，如果加载失败会抛出ClassNotFoundException异常。
				
		-	通过java.sql包中类DriverManager的静态方法getConnection(String url,String user,String password)建立数据库连接。
	
	-	向数据库发送SQL语句：要执行SQL语句首先要获得Statement类对象。通过连接数据库对象的createStatement()方法可获得Statement对象。
	
	-	处理查询结果集：有了Statement对象以后，可调用相应的方法实现对数据库的查询和修改，并将查询的结果集存放在Result类对象中。
