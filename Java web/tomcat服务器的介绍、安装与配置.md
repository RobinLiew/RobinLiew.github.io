## 基本介绍
- Tomcat服务器架构
![这里写图片描述](http://img.blog.csdn.net/20180301105223710?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- Tomcat目录
![这里写图片描述](http://img.blog.csdn.net/20180301105249633?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
	- bin：存放tomcat启动关闭所用的批处理文件
	- conf : tomcat的配置文件， 最重要的是server.xml，其中可以修改端口号
	- lib : tomcat运行所需的jar包
	- logs : tomcat运行时产生的日志文件
	- temp : tomcat运行时使用的临时目录，不需要关注
	- webapps : web应用所存放的根目录
	- work : tomcat的工作目录
- conf目录下的server.xml文件详解

```
<?xml version='1.0' encoding='utf-8'?>

<Server port="8005" shutdown="SHUTDOWN">
  <!--APR library loader. Documentation at /docs/apr.html -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!--Initialize Jasper prior to webapps are loaded. Documentation at /docs/jasper-howto.html -->
  <Listener className="org.apache.catalina.core.JasperListener" />
  <!-- Prevent memory leaks due to use of particular java/javax APIs-->
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

  <!-- 全局命名资源，用来定义一些外部访问资源，作用是为所有的引擎程序所引用的外部资源的定义-->
  <GlobalNamingResources>
    <!-- 定义一个名为“UserDatabase”的认证资源，将conf/tomcat-users.xml加载至内存中，在需要认证的时候到内存中进行认证-->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>

  <!-- 定义Service组件，同来关联Connector和Engine，一个Engine可以对应多个Connector，每个Service中只能一个Engine-->
  <Service name="Catalina">
	<!-- 修改HTTP/1.1的Connector默认监听端口80为8180.客户端通过浏览器访问的请求，只能通过HTTP传递给tomcat。  -->
    <Connector port="8180" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
    <!-- 当前Engine默认主机是localhost，当然你可以按照自己的需求修改  --> 
    <Engine name="Catalina" defaultHost="localhost">
	  <!-- Realm组件，定义对当前容器内的应用程序访问的认证，通过外部资源UserDatabase进行认证 -->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>
	  <!--  默认的主机域名为localhost，应用程序的目录是/webapps，设置自动解压，自动部署    -->
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
		<!-- 定义一个别名www.robinliew.com --> 
		<Alias>www.robinliew.com</Alias>   
		<!--  定义该应用程序，访问路径""，即访问www.robinliew.com即可访问，网页目录为：相对于appBase下的test1/，
				即/webapps/test1，并且当该应用程序下web.xml或者类等有相关变化时，自动重载当前配置，即不用重启tomcat使部署的新应用程序生效  -->		
        <Context path="" docBase="test1/" reloadable="true" />   
		<!--  定义另外一个独立的应用程序，访问路径为：www.robinliew.com/test2，该应用程序网页目录为/webapps/test2  --> 
        <Context path="/test2" docBase="/webapps/test2" reloadable="true" />   
        <!--   定义一个Valve组件，用来记录tomcat的访问日志，日志存放目录为：logs。
				定义日志文件前缀为localhost_access_log.并以.txt结尾，pattern定义日志内容格式，具体字段表示可以查看tomcat官方文档   --> 
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log." suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

      </Host>
    </Engine>
  </Service>
</Server>

```

- conf目录下的tomcat-users.xml文件
![这里写图片描述](http://img.blog.csdn.net/20180301105930559?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
##安装与配置
- 1.Tomcat的启动安装：
	- JAVA_HOME环境变量
	Tomcat需要运行在JVM虚拟机上，它通过JAVA_HOME找到JVM，因此将JAVA_HOME环境变量设置为JDK的主目录，就可以使用startup.bat文件启动Tomcat了
	- 端口占用问题
	可以使用netstat -ano指令查看，强行关闭
	- Catalina_home环境变量的设置问题
	
- 2.虚拟主机（一个真实主机可以运行多个网站，对于浏览器来说这些网站就好像都运行在自己的独立主机中一样，
	所以我们可以说每一个网站都运行在一个虚拟主机上，一个网站就是一个虚拟主机）
	
- 3.web应用（web资源不能直接交给虚拟主机，需要按照功能组织放进一个文件夹成为web应用再交给虚拟主机管理）
	
- 4.web应用的目录
	- 静态网页资源文件
	- WEB-INF文件夹
		- classes文件夹
		- lib文件夹
		- web.xml:将某个web资源配置为web应用首页
			
- 5.配置虚拟主机：
	- 在conf/server.xml中<Engine>标签下配置<Host>标签就可以为Tomcat增加一台虚拟主机了
	- name：指定虚拟主机的名称，浏览器通过这个名称访问虚拟主机
	- appBase:虚拟主机管理的目录，放置在这个目录下的web应用，当前虚拟主机可以自动加载
	- 由于浏览器访问地址时，需要将地址翻译成ip才能找到服务器，这其中的翻译过程由dns服务器来实现。
	我们没办法修改dns服务器，此时可以使用hosts文件模拟dns的功能，从而完成实验
	
- 6.缺省虚拟主机：如果访问是通过ip来访问，这个时候服务器无法辨别当前要访问的是哪台虚拟机中的资源
	，此时访问缺省虚拟主机，缺省虚拟主机可以在server.xml中的engin标签上通过defaultHost属性进行配置
	
- 7.为虚拟主机配置web应用：
	- 方法一：在Server.xml的<Host>标签中，配置<Context>标签，就可以为该虚拟主机配置一个web应用了
			如果将path设置为空则这个web应用为缺省应用
			这种配置方式需要重启服务器，不推荐
	- 方法二：在tomcat/conf/[Engin]/[Host]/在这个目录下写一个XML文件，其中xml文件的名字就是虚拟路径，
			如果是多级应用用#代替/表示，文件中配置<Context docBase="真实目录">，
			如果文件名起为ROOT.xml则此web应用为默认web应用
	- 方法三：直接将web应用放置到虚拟主机对应的目录下，如果目录名起为ROOT则此web应用为默认web应用