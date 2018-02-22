# 自己以前的学习笔记，希望可以帮助到一些学习Java web的人。还有一部分内容没写完，以后抽时间补上。
注意：图片可以右击点击查看图片，放大（使用火狐浏览器）
![这里写图片描述](http://img.blog.csdn.net/20180106163022914?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<html>
<head>
<META http-equiv="Content-Type" content="text/html; charset=UTF-8">
<meta content="text/html; charset=utf-8" http-equiv="Content-Type">
<meta content="text/css" http-equiv="Content-Style-Type">
<title>Java  Web基础</title>
</head>
<body>
<h1 align="center" class="root">
<a name="3lom1e5j736r5uur199n0plia9">Java  Web基础</a>
</h1>
<h2 class="topic">
<a name="232m1dm7gd4es3cvpinephqotg">一、Servlet</a>
</h2>
<h3 class="topic">
<a name="7j6f49fbov0c0ames6emgj2m9t">&nbsp;1.创建Servlet</a>
</h3>
<h3 class="topic">
<a name="1cl95fvn9nilac4067obs3rii6">&nbsp;&nbsp;1.实现Servlet接口</a>
</h3>
<h3 class="topic">
<a name="0uknjl7kc992oaufp2lukb7io9">&nbsp;&nbsp;2.继承javax.servlet.GenericServlet</a>
</h3>
<h3 class="topic">
<a name="7ai02ql6qfbtfgo8593vfntcst">&nbsp;&nbsp;3.继承javax.servlet.http.HttpServlet</a>
</h3>
<h3 class="topic">
<a name="6bfa8jtu2nlo00ckmtojchcsdk">&nbsp;2.Servlet生命周期</a>
</h3>
<h3 class="topic">
<a name="362hosloaauvfgpvfqeftdku4s">&nbsp;&nbsp;1.出生--&gt;init方法:在构造方法之后调用</a>
</h3>
<h3 class="topic">
<a name="271nos0qaql10hpjc4dq90u16t">&nbsp;&nbsp;2.使命--&gt;service方法：请求发过来时，处理请求使用</a>
</h3>
<h3 class="topic">
<a name="33lqf3uvro44ilrhtktfgs6voq">&nbsp;&nbsp;3.销毁--&gt;destory方法：服务器关闭时，会销毁Servlet，在销毁前调用该方法释放资源，销毁内存中的实例</a>
</h3>
<h3 class="topic">
<a name="764thv1au6458bhp6m4huojdt8">&nbsp;3.Servlet在web.xml中的注册</a>
</h3>
<h3 class="topic">
<a name="2n4oh9up3efcp9n9d74b77d315">&nbsp;&nbsp;1.注册Servlet到项目中</a>
</h3>
<h3 class="topic">
<a name="7p2n5617hanjufj791vevlpt80">&nbsp;&nbsp;2.分配给Servlet路径</a>
</h3>
<h3 class="topic">
<a name="0s6h696q16ejhb073vkn69o0r5">&nbsp;&nbsp;&nbsp;1.Servlet路径配置详解</a>
</h3>
<h3 class="topic">
<a name="5j1b0fmi9ssdu0hov4pplgige9">&nbsp;&nbsp;&nbsp;&nbsp;1.路径匹配：/ServletDemo,  /ABC/ServletDemo  ,/ABC/*</a>
</h3>
<h3 class="topic">
<a name="54ktcee0el80et3rpofr966v50">&nbsp;&nbsp;&nbsp;&nbsp;2.后缀名匹配：*.do  ,*.action  ,*.html</a>
</h3>
<h3 class="topic">
<a name="1j2v805656i49e7qu1gihepb3g">&nbsp;&nbsp;&nbsp;&nbsp;3.配置的路径匹配范围越大优先级越低</a>
</h3>
<h3 class="topic">
<a name="0ht48dvdbatsee0csfuro8njoh">&nbsp;4.访问Servlet的时序过程</a>
</h3>
<h3 class="topic">
<a name="5gus2oprq9t4tf46l1b9karbi8">&nbsp;&nbsp;1.一般情况下，Servlet在第一次被访问时于内存中创建对象，随后调用init方法进行初始化，对每一次请求都调用service方法进行处理，此时会用Request对象封装请求信息，并用Response对象（初始为null）代表响应消息，传入service方法供其使用。</a>
</h3>
<h3 class="topic">
<a name="18skgo3j09buc53g3ucjh3ok75">&nbsp;&nbsp;2.当service方法处理完后，返回服务器，服务器根据response中的信息组织响应消息，返回给浏览器。响应结束后，servlet并不会销毁，一直驻留在在内存中等待下一次请求，直到服务器关闭或web应用被移除。</a>
</h3>
<h3 class="topic">
<a name="5pam58nmjqfu0usbq5kh0bfdf9">&nbsp;5.Servlet的 线程安全问题：解决办法，使用局部变量保存用户数据</a>
</h3>
<h3 class="topic">
<a name="3aphd4c4okbv2uns6dl0jss36r">&nbsp;6.Servlet技术三大组件</a>
</h3>
<h3 class="topic">
<a name="19g5s9qe9jatcti70pg8ukk721">&nbsp;&nbsp;1.Servlet</a>
</h3>
<h3 class="topic">
<a name="49ip1vquf8g560qtm5t132nlm0">&nbsp;&nbsp;2.Filter过滤器</a>
</h3>
<h3 class="topic">
<a name="7uv2hjvv0njsp3fteq99c2jnnm">&nbsp;&nbsp;3.Listener监听器</a>
</h3>
<h2 class="topic">
<a name="2l77j370q57vdvcbqm4l0v9mdt">二、ServletConfig</a>
</h2>
<h3 class="topic">
<a name="70e5bskeuq7fs5ghgfv44rj0gp">&nbsp;1.代表Servlet的配置对象（可以在web.xml中配置）</a>
</h3>
<h3 class="topic">
<a name="18tioijddh4l573a05mkpgi2in">&nbsp;&nbsp;1.Servlet中可以用this.getServletConfig()获得该对象</a>
</h3>
<h3 class="topic">
<a name="5ek7aoe5o5kbulfim7re41qbmd">&nbsp;&nbsp;2.常用方法：getServletName()和getInitParameterNames()（可以遍历出配置中的配置项）</a>
</h3>
<h3 class="topic">
<a name="1rmqe7h05t1d6ra2hkibtt1atk">&nbsp;&nbsp;3.&lt;servlet&gt;&lt;init-param&gt;&lt;param-name&gt;&lt;/param-name&gt;&lt;param-value&gt;&lt;/param-value&gt;&lt;/init-param&gt;&lt;/servlet&gt;</a>
</h3>
<h3 class="topic">
<a name="66jt8n8vircubsuq1pnvv7ddn9">&nbsp;2.设置Servlet随着项目的启动而创建</a>
</h3>
<h3 class="topic">
<a name="2nht5j5a4ncnjvc9b63l4b6iob">&nbsp;&nbsp;1.&lt;servlet&gt;&lt;load-on-startup&gt;填写一个整数，整数越小优先级越高&lt;/load-on-startup&gt;&lt;/servlet&gt;</a>
</h3>
<h2 class="topic">
<a name="7r5e3dvgtc89jl2grh44r9bv59">三、ServletContext</a>
</h2>
<h3 class="topic">
<a name="3asd15fej53i3etv3nc48crrid">&nbsp;1.ServletContext代表对整个应用的配置，获得方法，getServletContext方法</a>
</h3>
<h3 class="topic">
<a name="0kneaglen1tjnqqfncad8p4kr5">&nbsp;2.ServletContext的作用</a>
</h3>
<h3 class="topic">
<a name="1n88o61a46hi2tumjk5bkem9u8">&nbsp;&nbsp;1.封装了web.xml中的配置</a>
</h3>
<h3 class="topic">
<a name="1aqs708csqg8u3k8p0sgprlc1u">&nbsp;&nbsp;2.Servlet技术中三大域对象之一。ServletContext  application；session；request</a>
</h3>
<h3 class="topic">
<a name="220ainhjph392ml7o2upcr26vv">&nbsp;&nbsp;3.获得项目中资源</a>
</h3>
<h3 class="topic">
<a name="43ic7mvi1kqn6ct7mkkl68sdgt">&nbsp;&nbsp;&nbsp;ServletContext sc=getServletContext();InputStream is=sc.getResourceAsStream("/WEB-INF/cities.xml");String path=sc.getRealPath("/WEB-INF/cities.xml")获取资源的绝对路径</a>
</h3>
<h3 class="topic">
<a name="3j51il8q5d956b3pdfvoopmei5">&nbsp;3.application域。原理：利用一个项目中只有一个ServletContext实例的特点，在ServletContext中放置了一个Map，这个Map 就是所谓的域。</a>
</h3>
<h2 class="topic">
<a name="4o5liropmfbm0l8t32b1c6128j">四、Response</a>
</h2>
<h3 class="topic">
<a name="4142r8slp4714n5a9m4c4btnng">&nbsp;1.response对象代表响应，我们在其中填入要发送到浏览器的内容，服务器把这些内容组装成http响应</a>
</h3>
<h3 class="topic">
<a name="0bf44khpf6u1on9q1a0rg39g4e">&nbsp;2.http响应及对应操作方法</a>
</h3>
<h3 class="topic">
<a name="00131811tu1sfk7uo37rt4h0i1">&nbsp;&nbsp;1.响应首行（协议/版本号  状态码  状态码描述）。添加状态码和描述的方法：void setStatus(int sc);  void setStatus(int sc,String sm);  void sendError(int sc);void sendError(int sc,String msg)</a>
</h3>
<h3 class="topic">
<a name="1tiktrae1hhmd175fjhj2ce3q1">&nbsp;&nbsp;2.响应头。添加响应头</a>
</h3>
<h3 class="topic">
<a name="34bal31in0nnv42l6848pri2fi">&nbsp;&nbsp;&nbsp;1.设置响应头，key一样会覆盖。void setHeader(String name,String value);  setIntHeader(String name,int value);  setDateHeader(String name,long date)</a>
</h3>
<h3 class="topic">
<a name="0m663ktjg6h2se1622skma548o">&nbsp;&nbsp;&nbsp;2.设置响应头，无论如何都新增。addHeader(String name,String value);  addIntHeader(String name,int value);  addDateHeader(String name,long date);</a>
</h3>
<h3 class="topic">
<a name="4hcaf7u0rn9t1362uqeo9rfneq">&nbsp;&nbsp;3.响应正文（响应空行）。发送字节流 getOutputStream;发送字符流getWriter</a>
</h3>
<h3 class="topic">
<a name="3r9lo2afu2uq9ljsqjgqq1uvor">&nbsp;4.重定向</a>
</h3>
<h3 class="topic">
<a name="5n7s17564c5v88eksdcr7ppv9k">&nbsp;&nbsp;1.response.setHeader("location","http://www.baidu.com")  or  response.setHeader("location","day1/ServletDemo")    +response.setStatus(302)</a>
</h3>
<h3 class="topic">
<a name="06bv45b2da7rg5u6mdcdbppgln">&nbsp;&nbsp;2.response.sendRedirect(String url);</a>
</h3>
<h3 class="topic">
<a name="1eogn0n9gc2lqg0hea4d21qrr8">&nbsp;5.乱码问题的解决</a>
</h3>
<h3 class="topic">
<a name="3prv5aq04ckj6b5sujfnr7rp6t">&nbsp;&nbsp;1.字节流。服务器端：string.getBytes("码表&ldquo;)   ;浏览器端：浏览器通过标签&lt;meta  http-equiv="content-type"  content="text/html;charset=utf-8"&gt;控制使用码表解码。对应的响应头：content-type：text/html;charset=utf-8。</a>
</h3>
<h3 class="topic">
<a name="44o4iahfc17528tqt8jqe7vv2q">&nbsp;&nbsp;2.字符流。PrintWriter pw=response.getWriter();  pw.print("你好世界")   服务端默认是使用ISO-8859-1码表。控制字符流使用码表response.setCharacterEncoding("UTF-8")（这个一般要放在最上面，因为得到流时要先取码表）</a>
</h3>
<h3 class="topic">
<a name="1tf9dbmb4pq8130lmpcmqgosdp">&nbsp;&nbsp;3.Java EE提供了一个方法，可以同时解决编码和解码的问题。response.setContextType("text/html;charset=utf-8");</a>
</h3>
<h3 class="topic">
<a name="08adfgb75niu7db4r85ep1da89">&nbsp;6.使用字节流发送图片</a>
</h3>
<h3 class="topic">
<a name="33pcegjc6vpabugs9oljvdcumv">&nbsp;&nbsp;1.告诉浏览器发给你流的MIME类型。response.setContentType("image/jpeg&ldquo;);</a>
</h3>
<h3 class="topic">
<a name="5bgvoo9pne6mhparn8o7mq70e6">&nbsp;&nbsp;2.获得图片输入流。InputStream in=getServletContext().getResourceAsStream("WEB-INF/1.jpg");</a>
</h3>
<h3 class="topic">
<a name="7qbsnrptbmahqb8fcssrvvffns">&nbsp;&nbsp;3.获得输出字节流。OutputStream out=response.getOutputStream();</a>
</h3>
<h3 class="topic">
<a name="6dg43h8en7avnrpb5prl8hogd6">&nbsp;&nbsp;4.对接输入输出流（略）</a>
</h3>
<h3 class="topic">
<a name="6jdnpcl5pqngg06vlf1ecjp6tl">&nbsp;7.使用字节流发送文件</a>
</h3>
<h3 class="topic">
<a name="7r89b1lhs2kte5e1mh8idfn294">&nbsp;&nbsp;1.文件的类型可以参考Tomcat中conf/web.xml中配置的MIME类型。例：&lt;mime-mapping&gt;&lt;extension&gt;jar&lt;/extension&gt;&lt;mime-type&gt;application/java-archive&lt;/mime-type&gt;&lt;/mime-mapping&gt;</a>
</h3>
<h3 class="topic">
<a name="0iufvdbrs595ujhm6e6k2g55sh">&nbsp;&nbsp;2.代码实现</a>
</h3>
<h3 class="topic">
<a name="54p993nn394drn4f3m143q5iag">&nbsp;&nbsp;&nbsp;1.告诉浏览器要发送的是什么。response.setContentType("application/java-archive");  引号中的内型可以通过getServletContext().getMimeType(".jar")获得（可以获得&ldquo;.jar&rdquo;的MIME类型）。</a>
</h3>
<h3 class="topic">
<a name="100sjra0rd6fs1pdca57ph78i3">&nbsp;&nbsp;&nbsp;2.告诉浏览器推荐用户使用什么名称下载（不设置的话默认文件名为路径名）。response.setHeader("content-disposition","attachment;filename="+filename); filename为String类型。</a>
</h3>
<h3 class="topic">
<a name="66brat2699nu7745s8nm3uf87q">&nbsp;&nbsp;&nbsp;3.开启输入输出流，并对接。</a>
</h3>
<h2 class="topic">
<a name="2811n9nfpj8tuf6g9pn8e2vr3u">五、Request</a>
</h2>
<h3 class="topic">
<a name="1ca3nscdtr4li26hd2n94l310b">&nbsp;1.http请求及对应操作方法</a>
</h3>
<h3 class="topic">
<a name="6ecargnmuf307r8sc0tngaiisq">&nbsp;&nbsp;1.请求行  请求方式  请求路径  协议/版本号。request.getMethod();GET    request.getRequestURI();/day12/ServletDemo    request.getServletPath();/day12    request.getScheme();http</a>
</h3>
<h3 class="topic">
<a name="188398cr31qptko0dbaq6i6lvh">&nbsp;&nbsp;2.请求头，request的方法。String getHeader(String name);  long getDateHeader(String name);  int getIntHeader(String name);  Enumeration getHeaders(String name);  Enumeration getHeaderNames();</a>
</h3>
<h3 class="topic">
<a name="0klisfqeu82qc67i3m7i7fqign">&nbsp;&nbsp;3.请求正文（请求空行之后），表单传递过来的键值对。</a>
</h3>
<h3 class="topic">
<a name="6at8vqd7s804sjl0vn7uq4j26t">&nbsp;2.GET提交</a>
</h3>
<h3 class="topic">
<a name="40rv2560ebl5agsbdqi8lq39hn">&nbsp;&nbsp;1.由于HTTP协议规定URL路径只能存在ASCII码中的字符，所以URL中存在中文或其他特殊的字符需要进行URL编码。原理：a.将空格转换为+；b.对0~9，a-z，A-Z之间的字符保持不变； c.用这个字符的当前字符集编码在内存中的十六进制表示，并在每个字节前加上一个百分号%</a>
</h3>
<h3 class="topic">
<a name="61otc3tnise199d29sobt4pibm">&nbsp;&nbsp;2.获得表单提交上来的键值对。String name=request.getParameter("name");  String age=request.getParameter("age");  </a>
</h3>
<h3 class="topic">
<a name="49houf3fnlspcrohg43d1a99ua">&nbsp;&nbsp;3.解决乱码。byte[ ] nameByte=name.getBytes("ISO-8859-1");  String newName=new String(nameByte,"UTF-8");</a>
</h3>
<h3 class="topic">
<a name="1352jqk3a9qislhqjso7mec5g6">&nbsp;&nbsp;4.服务器默认使用ISO-8859-1解码，如下的URIEncoding来决定解码码表（conf/web.xml）。&lt;connector port="8080" protocol="HTTP/1.1"  URIEncoding="UTF-8"  connectionTimeout="2000"  redirectPort="8443" /&gt;</a>
</h3>
<h3 class="topic">
<a name="2jc8uog74tdolg58aoa9sdngr1">&nbsp;3.POST提交</a>
</h3>
<h3 class="topic">
<a name="676873f3vrmhgpf33q67kheld7">&nbsp;&nbsp;1.因为POST解码是在第一次调用getParameter之前，那么那么解决乱码则需要在该方法之前设置编码。request.setCharacterEncoding("UTF-8");</a>
</h3>
<h3 class="topic">
<a name="281m5pm43jam5l8g50feod74hc">&nbsp;4.获得表单参数的方法</a>
</h3>
<h3 class="topic">
<a name="1iu2f08alqqdcunne2e4097ja9">&nbsp;&nbsp;1.String getParameter();根据键获得值，  Map getParameterMap();获得服务器保存表单参数的容器，  Enumeration getParameterNames();获得提交的所有键，  String[ ] getParameterValues(String name);根据键获得值</a>
</h3>
<h3 class="topic">
<a name="230j1q0pj1ugrls4rvftqnip4a">&nbsp;5.request请求转发和包含功能</a>
</h3>
<h3 class="topic">
<a name="6o7q23g69u19t3f4bgj87sbhmi">&nbsp;&nbsp;1.转发：一个Servlet处理完毕交给下面的Servlet（JSP）继续处理。作用：完成分工：Servlet处理业务，JSP完成显示功能。注意：转发的时候，从分工上讲Servlet中不要做输出正文的动作（没有效果）。</a>
</h3>
<h3 class="topic">
<a name="7ki7h41d3mgkufbuoucuaob6su">&nbsp;&nbsp;2.request.getDispatcher("  ").forward(request,response);  request.getDispatcher("  ").include(request,response);</a>
</h3>
<h3 class="topic">
<a name="5bal2rjpqggvd8ncafiq4be53s">&nbsp;6.request域</a>
</h3>
<h3 class="topic">
<a name="5q45cpeevg5ga1dq23hkqbq565">&nbsp;&nbsp;1.使用请求转发时，Servlet处理完数据，处理结果要交给JSP显示，可以使用request域将处理结果由Servlet带给JSP显示</a>
</h3>
<h3 class="topic">
<a name="141dohjpj811i6u5s4rrl7o0h7">&nbsp;&nbsp;2.常用的操作方法。setAttribute;  getAttribute;  getAttributeNames;  removeAttribute;</a>
</h3>
<h3 class="topic">
<a name="6iv2pvudn2qrqg3aha8veasece">&nbsp;&nbsp;3.request范围：一个request对应一个request域，有多少request就有多少request域</a>
</h3>
<h2 class="topic">
<a name="5vjpck0do5244m4qgdse92ah49">六、路径小结</a>
</h2>
<h3 class="topic">
<a name="3nd3samcbbavf0cg68s45m8dq2">&nbsp;1.客户端路径（给浏览器用的路径）</a>
</h3>
<h3 class="topic">
<a name="5gvcnln3adkjo49qf3i3lkfa6k">&nbsp;&nbsp;1.&lt;from action="  "&gt;;  &lt;a href="  "&gt;;  &lt;img src="  "&gt;;  response.sendRequestDispatcher("url");  Refresh:3;url="  "</a>
</h3>
<h3 class="topic">
<a name="77403a442b9aiirh9dpm3vdsr9">&nbsp;&nbsp;2.客户端路径的写法</a>
</h3>
<h3 class="topic">
<a name="7u68jtbkhq5tlj8qhukfcv8d9r">&nbsp;&nbsp;&nbsp;1.带"/"  :  "/" 表示相对主机。例如，表单所在页面路径为--&gt;http://localhost:8080/day12/index.jsp , "/"代表http://localhost:8080/  。在JSP页面等可以使用request.getContextPath()得到应用路径，这样移动index.jsp页面到其他应用目录下时，不用做多余的修改。</a>
</h3>
<h3 class="topic">
<a name="2as7u6edbr3kdn4mtmumbq7n6u">&nbsp;&nbsp;&nbsp;不带&ldquo;/"： 表示从当前目录中找（开发中尽量避免不带&rdquo;/"的情况）。例，表单所在页面路径为http://localhost:8080/day12/index.jsp表示为http://localhost:8080/day12/    。一旦移动该页面到其他应用，还要对路径做相应的修改，不推荐。</a>
</h3>
<h3 class="topic">
<a name="3qkoch9ft61njopc1o93sm1n7t">&nbsp;&nbsp;3.服务器端路径写法</a>
</h3>
<h3 class="topic">
<a name="5nk3jsdpjt7jjve01vnln16s32">&nbsp;&nbsp;&nbsp;1.&ldquo;/"相对于项目。例如http://localhost:8080/day12/ServletDemo中就是相对于如http://localhost:8080/day12/这一段。</a>
</h3>
<h2 class="topic">
<a name="2dv4mc2b4n2enoncfrco0qir8d">七、JSP概述</a>
</h2>
<h3 class="topic">
<a name="63rsldjoc3i2qlm44qa3nsv5l3">&nbsp;1.JSP(java server page)  可以在tomcat中的work/catalina/localhost/day12/org/apache/jsp中可以查看.jsp文件转化后的文件</a>
</h3>
<h3 class="topic">
<a name="3mh7eig0s94ao43k0npjnv6u9i">&nbsp;2.JSP的构成：HTML+JSP脚本（JAVA）+标签。HTML代码会使用out.write()拼接输出。</a>
</h3>
<h3 class="topic">
<a name="4e68vp69ruhntoocsg3i36cn5m">&nbsp;3.JSP中的脚本</a>
</h3>
<h3 class="topic">
<a name="1vujv8u7cuclctev8cev8uj54v">&nbsp;&nbsp;1.声明标签&lt;%   %&gt;, 标签中可以写Java代码，该脚本中写的Java代码会生成到JSP对应类中的service方法中。</a>
</h3>
<h3 class="topic">
<a name="0hcub6eo18b5su0n6pnijstr6i">&nbsp;&nbsp;2.表达式&lt;%= i%&gt;, 编译之后的代码：out.print(i);</a>
</h3>
<h3 class="topic">
<a name="4lkpmkae4javok8bvvqrdda7b1">&nbsp;&nbsp;3.声明标签&lt;%! int i=0; %&gt;，编译之后，脚本中的代码会出现在类中，可以使用该脚本定义全局变量和方法。</a>
</h3>
<h3 class="topic">
<a name="1s3a1tb6lmd12rsnlq7mfqiqmh">&nbsp;&nbsp;4.注释&lt;%-- --%&gt;, 被注释掉的内容不会参与编译。注意和&lt;!-- --&gt;html注释的区别。</a>
</h3>
<h2 class="topic">
<a name="6fbjlneupigqcrj4b6hbvf6a82">八、Cookie</a>
</h2>
<h3 class="topic">
<a name="4dvsg3jdqcnn8ojjtu5n8n8s74">&nbsp;1.cookie原理：让浏览器记住键值对，是向响应头中添加一下头即可set-cookie:name=robin;  浏览器记住之后，向服务器发送键值对，是在请求头中添加下面的信息cookie:name=robin。</a>
</h3>
<h3 class="topic">
<a name="55gb33bbfnvoed9likg8lshr2d">&nbsp;2.添加一个cookie到浏览器</a>
</h3>
<h3 class="topic">
<a name="7hj9hqpn5nbsmeq3bci75rgllk">&nbsp;&nbsp;1.新建一个cookie（键值对）。Cookie cookie=new Cookie("name","robin");</a>
</h3>
<h3 class="topic">
<a name="6n66kqnqhbflgk4i5hn8tpv38b">&nbsp;&nbsp;2.将cookie添加到响应中。response.addCookie(cookie);</a>
</h3>
<h3 class="topic">
<a name="48ilt5lpaah6bfpirtnspbsblq">&nbsp;3.让浏览器发送cookie到服务器</a>
</h3>
<h3 class="topic">
<a name="3fsmqq16jk7cnvom8s1fk56neh">&nbsp;&nbsp;1.获得所有浏览器发送的cookie。Cookie[ ] cookies=request.getCookies();</a>
</h3>
<h3 class="topic">
<a name="2mfr5jjsdu8l6nasl7jbrkp2gn">&nbsp;&nbsp;2.遍历并判断我们要找的cookie。if(cookies!=null&amp;&amp;cookies.length)&gt;0{for(Cookie c:cookies){if(c.getName().equals("name"){... ...}}}}</a>
</h3>
<h3 class="topic">
<a name="3qtf1f1fpn12uagefg710oie09">&nbsp;4.浏览器记录cookie的时间及其设置</a>
</h3>
<h3 class="topic">
<a name="3crl3eo4gdqnavd9s1rvi4g77v">&nbsp;&nbsp;1.默认是在会话期间有效（关闭浏览器，cookie就会被删除）</a>
</h3>
<h3 class="topic">
<a name="5frotaqdn9gtdi5vh1ofth2g16">&nbsp;&nbsp;2.有效时间的设置。Cookie cookie= new Cookie("age","18");  cookie.setMaxAge(60*60);//单位是秒。  cookie.setMaxAge(-1);//设置为-1，就是相当于默认的有效时间，浏览器关闭就消失。 cookie.setMaxAge(0);//表示cookie一发送到浏览器就消失了。注：可以利用有效时间为0这个特性进行删除cookie的操作。</a>
</h3>
<h3 class="topic">
<a name="76bavpqri7jqe7s43tffp8h33p">&nbsp;5.浏览器发送cookie的时机</a>
</h3>
<h3 class="topic">
<a name="5auqp2818944ngn5fhujbf457b">&nbsp;&nbsp;1.cookie默认的时机就是发送cookie的Servlet所在的目录。例：day13/    ，   访问路径如果是该cookie的子路径，浏览器就会把该cookie带给服务器，例：/day13/abcd/xxServlet</a>
</h3>
<h3 class="topic">
<a name="42chhs0g7vqaphsc29n1m7525d">&nbsp;&nbsp;2.访问特定Servlet发送cookie。cookie.setPath("day13/ServletDemo");</a>
</h3>
<h3 class="topic">
<a name="4p1u0aob2gcc580q25m3tf219m">&nbsp;6.cookie中的域</a>
</h3>
<h3 class="topic">
<a name="2j1jorftpucfj0mfb2c6epfspc">&nbsp;&nbsp;1.想要一下三个主机能共享一个cookie，www.robin.com;music.robin.com;basketball.robin.com,完成一下两步设置即可1，设置cookie的domain为".robin.com"，设置cookie的path为"/"。以上就是跨主机访问cookie</a>
</h3>
<h2 class="topic">
<a name="5lpln2kabbg91kr92j72f82nlk">九、Session</a>
</h2>
<h3 class="topic">
<a name="7ujq0qfpq7so2gfprtuqhs3obf">&nbsp;1.Session技术：服务器端保存会话信息的技术。原理：浏览器第一次访问服务器，服务器会在内存中开辟一个空间（session），并把该session对应的ID发送给浏览器，当下次浏览器再去访问服务器，会把SessionID交给服务器，服务器通过SessionID找到刚才开辟的空间。注：请求头中cookie:JessionID=......;...  ...</a>
</h3>
<h3 class="topic">
<a name="722p3t3j8v2ikqlb3auptcutb9">&nbsp;2.SessionID丢失的时间</a>
</h3>
<h3 class="topic">
<a name="7crn395a1imbv06o8tmoai8bf6">&nbsp;&nbsp;1.服务器让浏览器记住SessionID的cookie的默认过期时间是（-1），关闭浏览器cookie就丢失，也就是说cookie丢失，SessionID就丢失了。</a>
</h3>
<h3 class="topic">
<a name="7jggm7hucs4v3uiui11engceuq">&nbsp;3.session的方法</a>
</h3>
<h3 class="topic">
<a name="3e375gk2bpcgfuhe6uk91koevf">&nbsp;&nbsp;1.  获得session,request.getSession();</a>
</h3>
<h3 class="topic">
<a name="7jmtgen7qplfnmuavcoqu1mrjd">&nbsp;&nbsp;2.   4个操作Map的方法</a>
</h3>
<h3 class="topic">
<a name="3ml2g5gmsvl16kv19d0uruk1gs">&nbsp;&nbsp;3.其他方法。long getCreationTime();//获得创建时间，     String getId();//获得sessionID，     long getLastAccessedTime();//获得最后一次访问的时间，     int getMaxInactiveInterval();//获得最大存活时间（session），   void setMaxInactiveInterval(int interval);//设置最大存活时间，    void invalidate();//立刻销毁Session，  boolean isNew();//是否是新创建的Session。</a>
</h3>
<h3 class="topic">
<a name="1a5lg03csna0knv0de9468moq6">&nbsp;4.session最大有效时间的设置</a>
</h3>
<h3 class="topic">
<a name="4ae7uhajrupf93ptnldr1de2n7">&nbsp;&nbsp;1.默认30分钟。 Tomcat的web.xml的&lt;session-config&gt;标签中有该配置。</a>
</h3>
<h3 class="topic">
<a name="7emc78068gt3vbo1cf4e0kc462">&nbsp;&nbsp;2.session过期时间的修改</a>
</h3>
<h3 class="topic">
<a name="7k58944m44ff2icnci2c5919o6">&nbsp;&nbsp;&nbsp;1.修改Tomcat的web.xml的&lt;session-config&gt;标签，影响服务器中的所有项目</a>
</h3>
<h3 class="topic">
<a name="4d4ru2b9f39am6ah789tnvrblk">&nbsp;&nbsp;&nbsp;2.在项目的web.xml中加入&lt;session-config&gt;配置，影响的是当前项目</a>
</h3>
<h3 class="topic">
<a name="11nnvgt3i9obpamj6alpo1651j">&nbsp;&nbsp;&nbsp;3.通过setMaxInactiveInterval(int interval)方法设置，影响的是当前操作的Session</a>
</h3>
<h3 class="topic">
<a name="7ipcb08glpc71hq6b1uq1sv58f">&nbsp;5.钝化和激活</a>
</h3>
<h3 class="topic">
<a name="3ab9rjgnm6afuut9ksv9d8g6be">&nbsp;&nbsp;1.服务器内存消耗太大</a>
</h3>
<h3 class="topic">
<a name="2832gel5dl85dlgfdqip5r86pr">&nbsp;&nbsp;2.某些HTTPSession对象已经长时间不用了，但还不至于销毁</a>
</h3>
<h3 class="topic">
<a name="4cknforg26ih7765j2f99a6473">&nbsp;&nbsp;3.服务器重启了</a>
</h3>
<h3 class="topic">
<a name="0e9htrhqb1d5mt78m4vg4mk61u">&nbsp;6.URL重写</a>
</h3>
<h3 class="topic">
<a name="5t0sv2fk1c3c7tqn2fks0ep235">&nbsp;&nbsp;1.如果浏览器禁用cookie功能不能保存任何cookie，那么session技术要用cookie来保存SessionID，没有cookie的话如何操作？使用URL重写：将页面中所有的链接末尾都加上cookie id的参数（jessionid=...），这样用户点击链接访问网站时，通过URL把SessionID带到服务器，这样就解决了。</a>
</h3>
<h3 class="topic">
<a name="036nan6hvin7i8t3omghdp6loj">&nbsp;7.Servlet技术中三大域对象小结</a>
</h3>
<h3 class="topic">
<a name="7if8e5jii8l2960knkhovgabdd">&nbsp;&nbsp;1.application--&gt;ServletContext:范围是整个项目，只有一个ServletContext对象，所有的Servlet组件都能被访问到</a>
</h3>
<h3 class="topic">
<a name="5ogmhrhtjlas1fcqla2cbepp0h">&nbsp;&nbsp;2.session--&gt;HttpSession:范围是一次会话，一个没有保存SessionID的浏览器链接到服务器就会开启一个会话，有多少个浏览器访问就至少有多少个Session</a>
</h3>
<h3 class="topic">
<a name="32aa31vslhg6a8vbr17u6mjds3">&nbsp;&nbsp;3.request--&gt;HttpServletRequest:范围是一次请求之内，当前正在处理的请求有多少个就存在几个request域</a>
</h3>
<h2 class="topic">
<a name="11evl2iloeca8ud09kl38j9uq8">十、JSP指令元素</a>
</h2>
<h3 class="topic">
<a name="285nv9lf4dmrfnmg7omnfj81v5">&nbsp;1.JSP指令写法和分类</a>
</h3>
<h3 class="topic">
<a name="3ru4fnnloik3nni8ba4q4a66lf">&nbsp;&nbsp;1.写法：&lt;%@  指令  属性=&ldquo;  &rdquo; %&gt;</a>
</h3>
<h3 class="topic">
<a name="6a7qebs08ipdkqj4h4ced8j941">&nbsp;&nbsp;2.page指令标记</a>
</h3>
<h3 class="topic">
<a name="6c7q1hup5fdtpi6q58b1ae5hsi">&nbsp;&nbsp;&nbsp;1.page属性包含在"&lt;%@page"和"%&gt;"之间</a>
</h3>
<h3 class="topic">
<a name="7rre701slq9ont3rqt7ptnjutp">&nbsp;&nbsp;&nbsp;2.这些属性可以单独存在，也可以几个或多个同时使用</a>
</h3>
<h3 class="topic">
<a name="76n4slqgqlnl8cl4n4hum2796m">&nbsp;&nbsp;&nbsp;3.page指令用来定义JSP文件的全局属性</a>
</h3>
<h3 class="topic">
<a name="5ekqse61lk9qp83c1tfms2aoc0">&nbsp;&nbsp;&nbsp;4.在JSP页面中，只有import可以出现多次，其他属性都只能出现一次</a>
</h3>
<h3 class="topic">
<a name="6ojc3jf762okuj99egcn1g1eaj">&nbsp;&nbsp;3.page属性</a>
</h3>
<h3 class="topic">
<a name="1c8t1ka2tru4mkjs83087km5mn">&nbsp;&nbsp;&nbsp;1.language:描述当前页面使用的语言（目前取值只有Java）language="java"</a>
</h3>
<h3 class="topic">
<a name="4eabuld2i3d0c8dgoeoepsjcve">&nbsp;&nbsp;&nbsp;2.import:用于导入Java包或类的列表。唯一一个可以出现多次的属性。例：import="java.util.Date"</a>
</h3>
<h3 class="topic">
<a name="6e51htav54dqos4boum2lejs0k">&nbsp;&nbsp;&nbsp;3.pageEncoding:决定服务器读取JSP时，采用什么编码。pageEncoding="UTF-8"</a>
</h3>
<h3 class="topic">
<a name="3fk4q3s2kbe9s6sc5gn39s3qju">&nbsp;&nbsp;&nbsp;4.contentType="text/html;charset=UTF-8"响应浏览器时，告诉浏览器用什么码表解码。注：contentType和pageEncoding属性只需指定一个，另外一个会自动指定。</a>
</h3>
<h3 class="topic">
<a name="5paqblb50sq4a4n9eprj2tmqfv">&nbsp;&nbsp;&nbsp;5.buffer=&ldquo;8kb"：决定缓存的大小</a>
</h3>
<h3 class="topic">
<a name="544u44a26ubai92rmeibifv4m2">&nbsp;&nbsp;&nbsp;6.autoFlush="true"：如果缓存写满了，如果该属性为true，会将缓存中的内容自动输出到浏览器</a>
</h3>
<h3 class="topic">
<a name="4p1hm8g3r846vti1o278lvlrik">&nbsp;&nbsp;&nbsp;7.errorPage="  "：当JSP页面出现了异常，指定要跳转到的页面</a>
</h3>
<h3 class="topic">
<a name="7c7cbcc0r8a5fqf7danrmk8avu">&nbsp;&nbsp;&nbsp;8.isErrorPage="false":标识当前页面是否是处理错误的页面。拓展：错误页面可以使用下面的统一配置。&lt;error-page&gt;&lt;error-code&gt;500&lt;/error-code&gt;&lt;location&gt;/Error/ErrorDemo.jsp&lt;/location&gt;&lt;/error-page&gt;</a>
</h3>
<h3 class="topic">
<a name="65ro3n086ig2kpjcmjmmqgq1d9">&nbsp;&nbsp;&nbsp;9.session="true"（一般不要修改）：页面中是否使用session对象。如果为false，那么session内置对象会消失，默认为true</a>
</h3>
<h3 class="topic">
<a name="77nrds4mt6ludffcthv1i41mn7">&nbsp;&nbsp;&nbsp;10.extends=&ldquo;  &rdquo;：一般不用。JSP生成的Java文件继承哪个类。默认继承：org.apache.jasper.runtime.HttpJspBase</a>
</h3>
<h3 class="topic">
<a name="7bs9a9ddunkvnmf3vfa5pc90f3">&nbsp;&nbsp;&nbsp;11.isELIgnored:用来指定EL表达式语言是否被忽略。true则忽略，false则计算表达式的值</a>
</h3>
<h3 class="topic">
<a name="3r686h2nv442fskmnokbk4588e">&nbsp;&nbsp;4.include指令标记</a>
</h3>
<h3 class="topic">
<a name="2odh96efrm4k61ng6djncvmi8f">&nbsp;&nbsp;&nbsp;1.&lt;%@include  file="filename"%&gt;</a>
</h3>
<h3 class="topic">
<a name="33pq2gcbshrgv3u16h3kea5ps0">&nbsp;&nbsp;&nbsp;2.include指令的作用是在JSP页面中静态包含一个文件，同时由JSP解析包含的文件内容</a>
</h3>
<h3 class="topic">
<a name="5rsvlpjkinisi9fahobjfnrufa">&nbsp;&nbsp;&nbsp;3.不可以在file所指定的文件后面接任何参数。&lt;%@include  file="jw.jsp?nm=browser"</a>
</h3>
<h3 class="topic">
<a name="11rh6v0km0cte2n0iot6283bi7">&nbsp;&nbsp;5.taglib指令标记</a>
</h3>
<h3 class="topic">
<a name="7ntibibba3jn70qt9vvh071bbo">&nbsp;&nbsp;&nbsp;1.taglib指令用于在JSP页面中导入标签库。常用的标签库JSTL</a>
</h3>
<h3 class="topic">
<a name="1mc62v3ndgtqml46r89l6qpha4">&nbsp;&nbsp;&nbsp;2.常用属性。uri:标签文件的URL地址，prefix：标签组的命名空间前缀</a>
</h3>
<h2 class="topic">
<a name="0in3mjp4oi1a3il3hamnbj56ku">十一、JSP九大内置对象</a>
</h2>
<h3 class="topic">
<a name="3p4uc5v9322mn6e8ot89aou17d">&nbsp;1.九大内置对象。HTTPServletRequest  request;   HTTPServletResponse  response;  HTTPSession session;  ServletContext application;  ServletConfig config;  Throwable exception;           Object page;  JspWriter out;  pageContext pageContext</a>
</h3>
<h3 class="topic">
<a name="47pdiu9qrr8igll6pm1ja55t8p">&nbsp;2.page对象一般指向了Servlet的实例（一般不用）</a>
</h3>
<h3 class="topic">
<a name="4mhd2q7t1tg49uul0ce5n3pd2g">&nbsp;3.JSPWriter对象</a>
</h3>
<h3 class="topic">
<a name="140e7a3vlsb3sbqvva6j8o2lj7">&nbsp;&nbsp;1.JSP中都使用JSPWriter再向外输出内容</a>
</h3>
<h3 class="topic">
<a name="2m3ftc3ur0g2279b81hnoco60a">&nbsp;&nbsp;2.response.getWriter和JSPWriter的区别：response.getWriter的输出会出现在JSPWriter输出的前面；JSPWriter缓存会附加到response.getWriter缓存后，最终输出response.getWriter缓存。注：JSP中不要直接使用response.getWriter</a>
</h3>
<h3 class="topic">
<a name="07p99ab49dqnugqr0v9ekhgoiq">&nbsp;4.pageContext对象</a>
</h3>
<h3 class="topic">
<a name="0djirprpnhma1viv5qomj8b5jl">&nbsp;&nbsp;1.page域：范围只在当前页面中（4个域中最小的一个域）。常用方法：setAttribute( , );  getAttribute();  removeAttribute();</a>
</h3>
<h3 class="topic">
<a name="2fttokn3v5n6c851lgnc6a4clu">&nbsp;&nbsp;2.操作其他三个域。以操作request域为例。</a>
</h3>
<h3 class="topic">
<a name="4eb0tggjimeb1mqu0khlrfcali">&nbsp;&nbsp;&nbsp;1.存值pageContext.setAttribute("name","robin",pageContext.REQUEST_SCOPE);</a>
</h3>
<h3 class="topic">
<a name="6f7re4s8h9clqi0sfii3oclfp0">&nbsp;&nbsp;&nbsp;2.取值 pageContext.getAttribute("name",pageContext.REQUEST_SCOPE);</a>
</h3>
<h3 class="topic">
<a name="1plp809qlk4v6jkno8csd20mgr">&nbsp;&nbsp;&nbsp;3.遍历所有键pageContext.getAttributeNamesInScope(pageContext.REQUEST_SCOPE);</a>
</h3>
<h3 class="topic">
<a name="3ii9dg130ecq44acv9gi7ct6ea">&nbsp;&nbsp;&nbsp;4.删除一个值pageContext.removeAttribute("name",pageContext.REQUEST_SCOPE);</a>
</h3>
<h3 class="topic">
<a name="73gb7gsl9d0o9mg7cr2n23r1le">&nbsp;&nbsp;3.获得其他八个内置对象</a>
</h3>
<h3 class="topic">
<a name="24krhrksh97kmikqnnq2nk4qo9">&nbsp;&nbsp;&nbsp;1.getRequest();  getResponse();  getSession;  ...  ...</a>
</h3>
<h2 class="topic">
<a name="65bqk4iqlpge6caeulufp707gj">十二、JSP标签</a>
</h2>
<h3 class="topic">
<a name="6l587au07ninlgfkcfqfoalhgg">&nbsp;1.已经不怎么用了，暂时略过</a>
</h3>
<h2 class="topic">
<a name="2nmqmaegdm1lgr237i7heujg40">十三、EL表达式</a>
</h2>
<h3 class="topic">
<a name="3qf2e504m79ldvqsv5dq8ak7cb">&nbsp;1.功能：替换掉页面中的表达式(&lt;%=表达式 %&gt;)</a>
</h3>
<h3 class="topic">
<a name="3belrm4rqp8p3rjgkj2nm6nq9b">&nbsp;2.EL内置对象</a>
</h3>
<h3 class="topic">
<a name="4a896su0vdqh28bsmglhst0rg4">&nbsp;&nbsp;1.通过四大内置对象对四大域进行访问</a>
</h3>
<h3 class="topic">
<a name="34iuito71h4hef9hvederholco">&nbsp;&nbsp;&nbsp;1.包括requestScope;  sessionScope;  applicationScope;  pageScope</a>
</h3>
<h3 class="topic">
<a name="3cfea5si815lac9nqqmrfh7ce4">&nbsp;&nbsp;&nbsp;2.使用格式。${requestScope.name};  ${sessionScope.name};  ${applicationScope.name};  ${pageScope.name}  ,这些表达式表示从四大域中取出键为name的值。${name}从最小的域开始，找key为name的属性值。</a>
</h3>
<h3 class="topic">
<a name="6enao77fmf8kces3d9kplgofao">&nbsp;&nbsp;&nbsp;3.域操作示例</a>
</h3>
<h3 class="topic">
<a name="5sbv5u02k4rrjrsv09m0ngn9tk">&nbsp;&nbsp;&nbsp;&nbsp;1.${requestScope.user.name}和${requestScope.user['name']等价于&lt;%((User)request.getAttribute("user")).getName(); %&gt;</a>
</h3>
<h3 class="topic">
<a name="1192vlj1kgpk6b899q7ed7gn4m">&nbsp;&nbsp;&nbsp;&nbsp;2.例：先存值，&lt;%String[ ] array=new String[ ]{"jack","rose","jerry","tom"};  request.setAttribute("array",array);%&gt;     通过EL取值，${requestScope.array[1]}   --&gt;rose</a>
</h3>
<h3 class="topic">
<a name="14n78u26h9n3ckn0cjkhps15gc">&nbsp;&nbsp;2.其他内置对象</a>
</h3>
<h3 class="topic">
<a name="60k73n23juqvll3ajtj1nsa5th">&nbsp;&nbsp;&nbsp;1.param 和paramValues这两个对象封装了表单参数</a>
</h3>
<h3 class="topic">
<a name="51ntgj05ns71b3u2ntcq5bc8hr">&nbsp;&nbsp;&nbsp;2.header和headerValues封装了HTTP请求头</a>
</h3>
<h3 class="topic">
<a name="7bttim9tkrkka9ajkgo8ttaikb">&nbsp;&nbsp;&nbsp;3.initParam封装了web.xml中的配置</a>
</h3>
<h3 class="topic">
<a name="2aa3de327e24cc8nbl0v4p0hcp">&nbsp;&nbsp;&nbsp;4.pageContext封装了九大内置对象的pageContext</a>
</h3>
<h3 class="topic">
<a name="3vou9cv9hqjm62j49i2ejk7qtq">&nbsp;&nbsp;&nbsp;5.cookie封装了cookie信息</a>
</h3>
<h3 class="topic">
<a name="2btgvhn4s5ukb7onm65ld4qukl">&nbsp;3.EL表达式中支持比较运算符和逻辑运算符。${empty  p1}，    empty：判断一个对象是否为null，String是否为&ldquo; &rdquo;，集合中是否有元素</a>
</h3>
<h3 class="topic">
<a name="4c3ds75lasj1m4d30p3fgo7iul">&nbsp;4.EL函数库</a>
</h3>
<h3 class="topic">
<a name="33n04pfkn22hghu8cpsehclrel">&nbsp;&nbsp;1.作用：简化页面中静态方法的调用，使用EL函数代替Java代码</a>
</h3>
<h3 class="topic">
<a name="7p8v4bast6751dfselb90q0ir1">&nbsp;&nbsp;2.自定义EL函数库过程</a>
</h3>
<h3 class="topic">
<a name="3o8onpj6252euq9m15ib0563tp">&nbsp;&nbsp;&nbsp;1.定义工具类，在类中定义静态方法</a>
</h3>
<h3 class="topic">
<a name="6ic9h57h80ri2v55or2vdoku4e">&nbsp;&nbsp;&nbsp;2.填写配置文件xxx.tld放置到WEB-INF下</a>
</h3>
<h3 class="topic">
<a name="1a9d3bhgmj5vhsmds84vfteopg">&nbsp;&nbsp;&nbsp;3.配置文件中内容。参考笔记</a>
</h3>
<h3 class="topic">
<a name="7ne11g83la9ckdi9soc09vagvh">&nbsp;5.系统自带的EL函数</a>
</h3>
<h3 class="topic">
<a name="1g512fh9tsr9u9dmvb5l3isa2e">&nbsp;&nbsp;1.${fn:contains("hello","el")}  判断是否包含</a>
</h3>
<h3 class="topic">
<a name="5ekpjkafi26js8k9238ah7rj5s">&nbsp;&nbsp;2.${fn:endsWith("abcd","cd")}  判断是否以某字符串结尾</a>
</h3>
<h3 class="topic">
<a name="3si34mce88ehi26dddbbhahihr">&nbsp;&nbsp;3.${fn:escapeXml("&lt;font color='red'&gt;haha&lt;/font&gt;")}  自动将HTML关键字符转义</a>
</h3>
<h2 class="topic">
<a name="5f9892ap3e33arhkm5t5uan1ro">十四、自定义标签（简单标签）</a>
</h2>
<h3 class="topic">
<a name="5b7s3qgjf0c7s06p37g2r2gunk">&nbsp;1.标签作用：自定义标签属于JSP规范.替换掉JSP中的JSP脚本&lt;% %&gt;，实现一些简单的逻辑运算。</a>
</h3>
<h3 class="topic">
<a name="5v4c1jcv65hqoog7nih8gol960">&nbsp;2.标签开发步骤</a>
</h3>
<h3 class="topic">
<a name="7icuceoopk49qf3svkhgmqoqg0">&nbsp;&nbsp;1.编写一个类，实现一个接口javax.servlet.jsp.tagext.SimpleTag或者继承javax.servlet.jsp.tagext.SimpleTagSupport。具体例子参照笔记</a>
</h3>
<h3 class="topic">
<a name="4s7ujh2nr9okhvrv2buh10n944">&nbsp;&nbsp;2.在WEB-INF目录下，建立一个扩展名为tld(Tag Library Definition)的xml文件。注：tld文件在WEB-INF或者jar包的META-INF目录下，都能自动找到。配置文件的写法参照笔记</a>
</h3>
<h3 class="topic">
<a name="2c3adpn67tqnvm3fatv8564iet">&nbsp;&nbsp;3.在JSP中使用。引入&lt;%@ taglib uri="http://www.robinliew.com/simpletag"  prefix="xxx" %&gt;   ,使用时标签的写法：&lt;xxx:ShowTime/&gt;</a>
</h3>
<h2 class="topic">
<a name="4qgqjemnt67k7jdt5qb1a3m8a8">十五、JSTL标签库</a>
</h2>
<h3 class="topic">
<a name="6tcq3oavg99i6lcku3e51n651v">&nbsp;1.JSTL简单介绍</a>
</h3>
<h3 class="topic">
<a name="561bs4spitbl2l0gs3rb65hkeq">&nbsp;&nbsp;1.JSTL：Java Standard Tag Library。Apache实现的一套标准的标签库(可访问JCP.org)</a>
</h3>
<h3 class="topic">
<a name="6u00j8b7fc2s5q4u1ngs4djkpa">&nbsp;&nbsp;2.由五部分组成：Core:核心    Functions:EL函数     Format：国际化    SQL：操作数据库    Xml：操作xml</a>
</h3>
<h3 class="topic">
<a name="0dm6nbhbtefom0r2mi0l3ep0ji">&nbsp;&nbsp;3.常用的core标签</a>
</h3>
<h3 class="topic">
<a name="3h16kk0fbs0m6enf4igkdn60ak">&nbsp;&nbsp;&nbsp;1.c:out 输出数据到页面上。&lt;c:out  value="${p}"  default="木有&ldquo;" esccpeXml="true"&gt;&lt;/c:out&gt;  其中escapeXml属性表示是否转义。</a>
</h3>
<h3 class="topic">
<a name="3n1hul39g1jo72gr0hhn647s0p">&nbsp;&nbsp;&nbsp;2.c:if    如果test表达式的值为 true 则执行其主体内容var属性：记住test表达式的运行结果；  scope：page（默认）|request|session|application 。例，&lt;c:if  test="${1==1}"   var="result"&gt;相等&lt;/c:if&gt;  。</a>
</h3>
<h3 class="topic">
<a name="5mu8002sgeqdvbmr5q72be0ian">&nbsp;&nbsp;&nbsp;3.c:choose    c:when     c:otherwise组合起来模拟了if  else  if</a>
</h3>
<h3 class="topic">
<a name="6lk0a73uudr9njrb7jhvvcomsu">&nbsp;&nbsp;&nbsp;4.c:forEach   模拟了for循环。迭代：数组，集合，Map和String（用逗号分隔）</a>
</h3>
<h3 class="topic">
<a name="49fac88bem6rofgl2hpc1rvvcd">&nbsp;&nbsp;&nbsp;5.c:url   对地址进行url重写，还能进行url编码</a>
</h3>
<h3 class="topic">
<a name="74nte3k14v71evjonetd578oro">&nbsp;&nbsp;&nbsp;6.c:import  动态包含，可以包含任何页面</a>
</h3>
<h3 class="topic">
<a name="5om6or0155kh2vb1gn4qnp2eso">&nbsp;&nbsp;&nbsp;7.c:forTokens  用分隔符隔离字符串</a>
</h3>
<h3 class="topic">
<a name="3o1q5hpojbe97tma114prl2eu0">&nbsp;&nbsp;&nbsp;8.c:set  用于设置变量值和对象属性</a>
</h3>
<h3 class="topic">
<a name="5v1t4rbge0pn4kdtc7reajehoa">&nbsp;&nbsp;&nbsp;9.具体用法可以参照笔记或网站http://www.runoob.com/jsp/jsp-jstl.html</a>
</h3>
<h3 class="topic">
<a name="2a609os3k41dfjf4nk8jbosbpd">&nbsp;2.JSTL自定义标签</a>
</h3>
<h2 class="topic">
<a name="4phf63keua0t9v275pdsrck24o">十六、SQL</a>
</h2>
<h3 class="topic">
<a name="6n3pkslfvrufn8004f2qdeqd1r">&nbsp;1.SQL：Structured Query Language 结构化查询语言。作用：是一种定义、操作、管理关系数据库的句法。</a>
</h3>
<h3 class="topic">
<a name="0cc6rgqmtm51kidgn7lhd0l67h">&nbsp;2.SQL语句的组成。DQL：结构查询语言     DML：数据操作语言     DDL：数据定义语言    DCL：数据控制语言     TPL：事务处理语言    CCL：指针控制语言</a>
</h3>
<h3 class="topic">
<a name="6mmv1rakc9347gbaoeck7gh2br">&nbsp;3.关系性数据库</a>
</h3>
<h3 class="topic">
<a name="36n553k58hkt72u5dti8q38h7i">&nbsp;&nbsp;1.属性：用来描述所在列的项目的语义</a>
</h3>
<h3 class="topic">
<a name="4u31m9904b1bftqrnvs5uvu1a2">&nbsp;&nbsp;2.模式：关系名和其属性集合的组合称为这个关系的模式。例，Movies(title,year,length,genre)。在关系模型中，数据库是由一个或多个关系组成。</a>
</h3>
<h3 class="topic">
<a name="1cj7agq586prcqckbna019g5ij">&nbsp;&nbsp;3.元组：关系中除含有属性名所在行以外的其他行称为元组（turple）。每个元组均有一个分量（component）对应于关系的每个属性。</a>
</h3>
<h3 class="topic">
<a name="3g1r6r3l572e2llagh11pavrrv">&nbsp;&nbsp;4.原子性。关系模型要求元组的每个分量具有原子性。也就是说，它必须属于某种元素类型，如Integer或String，而不能是记录，集合，数组或者其他任何可以分解成更小分量的组合类型。</a>
</h3>
<h3 class="topic">
<a name="0sdnu652omhrq5naikaopeq473">&nbsp;&nbsp;5.域：与关系的每个属性相关联的是一个域（domain），即一个特殊的元素类型，关系中任一元组的分量值必须属于对应列的域。例，关系Movies中四个分量对应的域分别是：String，integer，integer，String。Movies(title:string,year:integer,length:integer,genre:string)</a>
</h3>
<h3 class="topic">
<a name="5i3898eciviar5vt1qhrj7aqbf">&nbsp;&nbsp;6.关系实例：一个给定关系中元组的集合叫做关系的实例。关系的属性次序可以任意排列，关系不会改变，但重新排序关系模式时，要记住属性是列标题。</a>
</h3>
<h3 class="topic">
<a name="3p3199fnici1vbitqutucc16vg">&nbsp;&nbsp;7.键约束：键由关系的一组属性集组成，通过定义键可以保证关系实例上任何两个元组的值在定义键的属性集上取值不同。通常在形成键的属性或属性组下面画上下划线，用来表明它是键的组成部分。</a>
</h3>
<h3 class="topic">
<a name="6j3lult874t03ftpik0beallkd">&nbsp;4.SQL中的三类关系</a>
</h3>
<h3 class="topic">
<a name="7h18q3f5njnrnbianhn16vienl">&nbsp;&nbsp;1.存储关系，称为表（table）。这是通常要处理的一种关系，它在数据库中存储，用户能够对其元组进行查询和更新。</a>
</h3>
<h3 class="topic">
<a name="29a24jagflum3n7cb5c2p3ojab">&nbsp;&nbsp;2.视图（view），通过计算来定义的关系。这种关系并不在数据库中存储，它只是在需要的时候被完整或部分地构造。</a>
</h3>
<h3 class="topic">
<a name="4k8fgr7c2gtnancva900c192tm">&nbsp;&nbsp;3.临时表，它是在执行数据查询和更新时由SQL处理程序临时构造。这些临时表会在处理结束后被删除而不会存储在数据库里。</a>
</h3>
<h3 class="topic">
<a name="5qfs362f7rsscgk8835ppg606k">&nbsp;5.SQL中的数据类型</a>
</h3>
<h3 class="topic">
<a name="6fshm083ocdkfvo7f12r41o0ug">&nbsp;&nbsp;1.可变长度或固定长度的字符串</a>
</h3>
<h3 class="topic">
<a name="2edrmfvtd00mcu716crcns6lev">&nbsp;&nbsp;&nbsp;1.CHAR(n)：最大为n个字符的固定长度字符串</a>
</h3>
<h3 class="topic">
<a name="57f2cc3qbo289fsiki63rt8358">&nbsp;&nbsp;&nbsp;2.VARCHAR(n)：也表示最多可能有n个字符的字符串</a>
</h3>
<h3 class="topic">
<a name="4p7b3ju9v78jbqggcrqcktu2r9">&nbsp;&nbsp;&nbsp;3.区别：CHAR类型会以一些短的字符串来填充后面未满的空间来构成几个字符，而VARCHAR会使用一个结束符或字符长度值来标志字符串的结束，后面未满的空间不会做填充。</a>
</h3>
<h3 class="topic">
<a name="3ma2g1h8m672afqbcvo3qliju6">&nbsp;&nbsp;&nbsp;4.注：SQL中字符串是使用单引号</a>
</h3>
<h3 class="topic">
<a name="48slm3opk9f8ebggbe3ji4sv5t">&nbsp;&nbsp;2.类型INT和INTEGER表示典型的整数值。SHORTINT也表示整数，但表示的位数可能小一些，具体取决于实现。</a>
</h3>
<h3 class="topic">
<a name="1b1gm944iqmpdlnj9foo0n3vo8">&nbsp;&nbsp;3.浮点值能通过不同的方法表示</a>
</h3>
<h3 class="topic">
<a name="0vk0bfhssij9vmb3uunq03u1r2">&nbsp;&nbsp;&nbsp;1.FLOAT和REAL（同义词）：表示典型的浮点数值</a>
</h3>
<h3 class="topic">
<a name="5dup5cliq531hufetriaj7h80m">&nbsp;&nbsp;&nbsp;2.DOUBLE，PRECISION：高精度的浮点类型</a>
</h3>
<h3 class="topic">
<a name="496ulsfubj6jet715lailr264f">&nbsp;&nbsp;&nbsp;3.DECIMAL(n,d)：允许可以有n位有效数字的十进制，小数点是在右数第d位的位置。</a>
</h3>
<h3 class="topic">
<a name="2ljv819qi0v0p0taj0rnh44gcp">&nbsp;&nbsp;4.BOOLEAN表示具有逻辑类型的值。属性值是TRUE，FALSE，UNKNOWN</a>
</h3>
<h3 class="topic">
<a name="4309k5q36u823ti37v3jim1hoi">&nbsp;&nbsp;5.时间和日期分别通过TIME和DATA数据类型表示。例，TIME '15:00:02.5'    DATE '1948-05-14'</a>
</h3>
<h3 class="topic">
<a name="5p437of4fvg5g7f8od5kj55cn4">&nbsp;&nbsp;6.mysql和Java数据类型对比</a>
</h3>
<h3 class="topic">
<a name="1auefbc7oke3mr7c0prpd2b4o7">&nbsp;&nbsp;&nbsp;1.char(n)/varchar(n) ----String</a>
</h3>
<h3 class="topic">
<a name="0db7bb0kvk194jq7a9uemh5j3s">&nbsp;&nbsp;&nbsp;2.tinyint ----byte</a>
</h3>
<h3 class="topic">
<a name="4e2cbmtesmtd7rup2vokgkjpdl">&nbsp;&nbsp;&nbsp;3.smallint ----short</a>
</h3>
<h3 class="topic">
<a name="2j147gq1mmp1ci07gaf5osnag7">&nbsp;&nbsp;&nbsp;4.int ----int</a>
</h3>
<h3 class="topic">
<a name="6gjisl7hla93rfbascipssjfm1">&nbsp;&nbsp;&nbsp;5.bigint ----long</a>
</h3>
<h3 class="topic">
<a name="6picnlattt0gijc04i2baanbf7">&nbsp;&nbsp;&nbsp;6.double ----double</a>
</h3>
<h3 class="topic">
<a name="3ocjaj9fskmuq38pta1b3rbh17">&nbsp;&nbsp;&nbsp;7.bit(0/1) ----boolean</a>
</h3>
<h3 class="topic">
<a name="00vdg0ntv32vksl3f68lem35lk">&nbsp;&nbsp;&nbsp;8.Date/Time/DateTime/timeStamp ----Date</a>
</h3>
<h3 class="topic">
<a name="5u9vhmjmtfmn2tmkskh0q3gpdb">&nbsp;&nbsp;&nbsp;9.Blob/Text(大数据：图片，音频，视频二进制码；文本大数据)</a>
</h3>
<h3 class="topic">
<a name="0cl31dtchlpmcs200vhgr2il2t">&nbsp;6.DDL数据定义语言（对数据库的操作）</a>
</h3>
<h3 class="topic">
<a name="5p2m762ebd2d5f0adumumkcouc">&nbsp;&nbsp;1.常用关键字：create/alter/drop/truncate</a>
</h3>
<h3 class="topic">
<a name="266ji1fsash8jfcprj3dbj9pun">&nbsp;&nbsp;2.显示所有数据库：show databases;</a>
</h3>
<h3 class="topic">
<a name="1tekapd2amrvl7f032u6re08rb">&nbsp;&nbsp;3.创建一个名称为mydb1的数据库：create databases mydb1;</a>
</h3>
<h3 class="topic">
<a name="6991nnkpc8inhs0hkpqvdm1kr5">&nbsp;&nbsp;4.创建一个使用gbk字符集的mydb2数据库：create databases mydb2 character set gbk;</a>
</h3>
<h3 class="topic">
<a name="51eakbo62h09ieegojuuhovvtt">&nbsp;&nbsp;5.创建一个使用gbk字符集，并带校对规则的mydb3数据库：create database mydb3 character set gbk collate gbk_chinese_ci;</a>
</h3>
<h3 class="topic">
<a name="18qbq3i1m1sf4bvn6hlunpgkqt">&nbsp;&nbsp;6.查看数据库的创建细节，可以看到使用的字符集：show create database mydb1;</a>
</h3>
<h3 class="topic">
<a name="3k9rokgi1abt7i32ik9ce49vjf">&nbsp;&nbsp;7.删除当前创建的mydb3数据库：drop database mydb3;</a>
</h3>
<h3 class="topic">
<a name="6k73t9qa1sstik61ft1kt9qoge">&nbsp;&nbsp;8.查看服务器中的数据库，并把mydb2的字符集修改为utf8：alter database mydb2 character set utf8;</a>
</h3>
<h3 class="topic">
<a name="7mjqfksg4j3bl0q8iopd4n0noa">&nbsp;&nbsp;9.设置服务器端使用的编码：set character_set_client=gbk;</a>
</h3>
<h3 class="topic">
<a name="59m0u2ajpiog6qcadgvd5vfu51">&nbsp;&nbsp;10.查看数据库中的各种字符编码：show variables like 'character%' ;</a>
</h3>
<h3 class="topic">
<a name="0u8vo5sju922n9n870aho7pit3">&nbsp;7.DML数据操作语言</a>
</h3>
<h3 class="topic">
<a name="3j4cj1esqmv4vdpepn06nq4eiu">&nbsp;&nbsp;1.常用关键字：insert/update/delete</a>
</h3>
<h3 class="topic">
<a name="295t9ht5tt51lt2cii5h398vc4">&nbsp;&nbsp;2.表操作</a>
</h3>
<h3 class="topic">
<a name="15i4806bqld89hc2tjoglb4h1s">&nbsp;&nbsp;&nbsp;1.表操作前的准备工作：显示当前的数据库和选择数据库。</a>
</h3>
<h3 class="topic">
<a name="4g8enkakcjfrif7ts9vcbd394i">&nbsp;&nbsp;&nbsp;&nbsp;1.显示当前的数据库：select database();  </a>
</h3>
<h3 class="topic">
<a name="5ma6i76c61nte1vh67v609s5op">&nbsp;&nbsp;&nbsp;&nbsp;2.选择数据库：use mydb1;   创建表前，要先使用use db 语句使用库。</a>
</h3>
<h3 class="topic">
<a name="5qipsq4fgochtgucd9o4uue5j6">&nbsp;&nbsp;&nbsp;2.创建表</a>
</h3>
<h3 class="topic">
<a name="4198utrttosg7fp9919hf650gq">&nbsp;&nbsp;&nbsp;&nbsp;1.格式： create table tab_name( field1 type, field2 type, ... ,fieldn type)[character set xxx] [collate xxx];</a>
</h3>
<h3 class="topic">
<a name="3p46jquje60nbj27kdv61bdb4a">&nbsp;&nbsp;&nbsp;&nbsp;2.创建一个员工表employee。create table employee( id int primary key auto_increment, name varchar(20), gender bit default 1, birthday date, job varchar(20), salary double);</a>
</h3>
<h3 class="topic">
<a name="6ct611ve9jfomp3nq750fa1jab">&nbsp;&nbsp;&nbsp;3.查看表信息</a>
</h3>
<h3 class="topic">
<a name="7f0kuik1tj24fmuveub070c35f">&nbsp;&nbsp;&nbsp;&nbsp;1.desc tab_name 查看表结构</a>
</h3>
<h3 class="topic">
<a name="47mahv85ct6tu1bqbkvrf4sl2u">&nbsp;&nbsp;&nbsp;&nbsp;2.show tables 查看当前数据库中的所有表</a>
</h3>
<h3 class="topic">
<a name="459dgfuitcoptvffb0jrr78sgg">&nbsp;&nbsp;&nbsp;&nbsp;3.show create table tab_name 查看当前数据库建表语句</a>
</h3>
<h3 class="topic">
<a name="6fkbqipupbno4q33ds1cj1s5ul">&nbsp;&nbsp;&nbsp;4.修改表结构</a>
</h3>
<h3 class="topic">
<a name="30vel6m3c2avt6nbgud4fbj8co">&nbsp;&nbsp;&nbsp;&nbsp;1.增加一列：alter table tab_name add [column] 列名 类型;</a>
</h3>
<h3 class="topic">
<a name="1hmsian5kb2v6inp8ip0438tf9">&nbsp;&nbsp;&nbsp;&nbsp;2.修改一列类型：alter table tab_name modify 列名 类型;</a>
</h3>
<h3 class="topic">
<a name="69njod6cj6qnjpm8jif3abnroa">&nbsp;&nbsp;&nbsp;&nbsp;3.修改列名：alter table tab_name change [column] 列名 新列名 类型;</a>
</h3>
<h3 class="topic">
<a name="7e9k7im5l92cuss0aajar6nv18">&nbsp;&nbsp;&nbsp;&nbsp;4.删除一列：alter table tab_name drop [column] 列名;</a>
</h3>
<h3 class="topic">
<a name="46ea4hd9opjqs9bfi99ocg0oac">&nbsp;&nbsp;&nbsp;&nbsp;5.修改表明：rename table 表名 to 新表名;</a>
</h3>
<h3 class="topic">
<a name="4ti6pnj6c9e0mv847itqr7kgra">&nbsp;&nbsp;&nbsp;&nbsp;6.修改表所用的字符集：alter table 表明 character set utf8;</a>
</h3>
<h3 class="topic">
<a name="0k9huinov10b2u7c1gi4jh062j">&nbsp;&nbsp;&nbsp;&nbsp;7.删除表：drop table tab_name;</a>
</h3>
<h3 class="topic">
<a name="0kmldg7030qrtiblbg67hk9fuh">&nbsp;&nbsp;3.表的常用操作：增删改</a>
</h3>
<h3 class="topic">
<a name="5vtrfvjc2es0vhpi4atlnrv2eo">&nbsp;&nbsp;&nbsp;1.插入一条记录：insert into tab_name ( field1, field2, ...) values (value1, value2, ...);</a>
</h3>
<h3 class="topic">
<a name="7n38r3ie1rlo4ijhnai98mc5ps">&nbsp;&nbsp;&nbsp;&nbsp;1.插入的数据应与字段的的数据类型相同</a>
</h3>
<h3 class="topic">
<a name="0ltne1m5bkd99h58mgdeh9uegc">&nbsp;&nbsp;&nbsp;&nbsp;2.数据的大小应在列的规定范围内。例如，不能将一个长度为80的字符串加入到长度为40的列中</a>
</h3>
<h3 class="topic">
<a name="62hb736mi54qirc27gok9opom1">&nbsp;&nbsp;&nbsp;&nbsp;3.在values中列出的数据位置必须与被加入的列的排列位置相对应</a>
</h3>
<h3 class="topic">
<a name="3rhjv9vnd5vfms9qhmkm83hsor">&nbsp;&nbsp;&nbsp;&nbsp;4.字符和日期数据应该包含在单引号中</a>
</h3>
<h3 class="topic">
<a name="72iq6lan3q2q9trur1h2o10arc">&nbsp;&nbsp;&nbsp;&nbsp;5.不指定某列的值或insert into table (null)，则该列取空值</a>
</h3>
<h3 class="topic">
<a name="7lutd92pfrc9508coj2nd6tn6i">&nbsp;&nbsp;&nbsp;&nbsp;6.如果要插入所有字段可以省略列列表，直接按表中字段顺序写值。</a>
</h3>
<h3 class="topic">
<a name="3jn363gd5ndr34vu1oiihe7koj">&nbsp;&nbsp;&nbsp;2.修改表记录：update tab_name set field1=value1, field2=value2, ... [where语句];</a>
</h3>
<h3 class="topic">
<a name="1ooomupg46ubo0mv8fphb5ccrj">&nbsp;&nbsp;&nbsp;&nbsp;1.set子句指示要修改哪些列和要给予哪些值</a>
</h3>
<h3 class="topic">
<a name="2onqh8vt4f0fj837u30lhopetq">&nbsp;&nbsp;&nbsp;&nbsp;2.where 子句指定应更新哪些行。如果没有where子句，则更新所有行。</a>
</h3>
<h3 class="topic">
<a name="126qsfctnorre2vl0v64o4g2ag">&nbsp;&nbsp;&nbsp;&nbsp;3.例：将姓名为'zs'的员工薪水改为3000元。update emp set salary=300 where name='zs';    将姓名为&lsquo;xiaoliu'的员工薪水改为5000元，job改为enginer：update emp set salary=5000, job=enginer where name='xiaoliu';</a>
</h3>
<h3 class="topic">
<a name="36v9fmu6fc5r0qfirih8r4tf2n">&nbsp;&nbsp;&nbsp;3.删除表操作：delete from tab_name [where ...];</a>
</h3>
<h3 class="topic">
<a name="2155jlr80eqj5m7gplfrvhv7h3">&nbsp;&nbsp;&nbsp;&nbsp;1.如果不跟where子句则删除整张表中的数据</a>
</h3>
<h3 class="topic">
<a name="3v2thjha46pk4cedkksj8ofji0">&nbsp;&nbsp;&nbsp;&nbsp;2.delete只能用来删除一行记录，不能删除一行记录中的某一列值</a>
</h3>
<h3 class="topic">
<a name="22tq8dh9480c7nhcdlku5htivd">&nbsp;&nbsp;&nbsp;&nbsp;3.delete语句只能删除表中的内容，不能删除表本身，要想删除表，用drop</a>
</h3>
<h3 class="topic">
<a name="73r6lntbt3bkd0f63cghdstj6v">&nbsp;&nbsp;&nbsp;&nbsp;4.truncate table 可以删除表中所有数据，此语句首先摧毁表，再新建表。此种方式删除的数据不能再从事务中恢复。</a>
</h3>
<h3 class="topic">
<a name="1h6o45ujl5t75egnm9r283jghp">&nbsp;&nbsp;&nbsp;&nbsp;5.例，删除表中名称为'zs'的记录。delete from emp where name='zs';</a>
</h3>
<h3 class="topic">
<a name="2g4lb7n4mb1vbialjrl1o81h9f">&nbsp;8.DQL数据查询语言</a>
</h3>
<h3 class="topic">
<a name="1qq0vd42udd7q719lf7dnmslp1">&nbsp;&nbsp;1.查询表操作（select）</a>
</h3>
<h3 class="topic">
<a name="4d4umv8mfcmoulmf08mepk6lc3">&nbsp;&nbsp;&nbsp;1.select [distinct] * | field1,field2, ... from tab_name;  其中distinct用来剔除重复行。例，select name,english from exam; select distinct english from exam;</a>
</h3>
<h3 class="topic">
<a name="6g0mqiua06hm9p7856dpv8u5mh">&nbsp;&nbsp;&nbsp;2.select可以使用表达式，且可以使用as 别名</a>
</h3>
<h3 class="topic">
<a name="5og17fukrmhdekpa20374mslhf">&nbsp;&nbsp;&nbsp;&nbsp;1.select name,english+10,chinese+10,math+10 from exam;</a>
</h3>
<h3 class="topic">
<a name="0phv3jvceoggrjofmnna9o2s0v">&nbsp;&nbsp;&nbsp;&nbsp;2.select name,english+chinese+math as 总成绩 from exam;</a>
</h3>
<h3 class="topic">
<a name="455cnsjn68thavteln6a0ori1b">&nbsp;&nbsp;&nbsp;&nbsp;3.select name,english+chinese+math  总成绩 from exam;</a>
</h3>
<h3 class="topic">
<a name="710rpq0hsec4lt0jfa4p3p4fu6">&nbsp;&nbsp;2.使用where子句进行过滤查询</a>
</h3>
<h3 class="topic">
<a name="3ropa4emu9ocsnqehe8qc55cc6">&nbsp;&nbsp;&nbsp;1.查询姓名为'xx'的学生成绩</a>
</h3>
<h3 class="topic">
<a name="7v5ao7iq2reenhko6o6j3ltuiv">&nbsp;&nbsp;&nbsp;&nbsp;1.select * from exam where name='xx';</a>
</h3>
<h3 class="topic">
<a name="4lj2mdhjmrun0acehajj7kvcm6">&nbsp;&nbsp;&nbsp;2.where子句支持的格式</a>
</h3>
<h3 class="topic">
<a name="6tbu698tknb9mu8hktlpimjq4f">&nbsp;&nbsp;&nbsp;&nbsp;1.比较运算符：&gt;  &lt;  &gt;=  &lt;=  &lt;&gt;</a>
</h3>
<h3 class="topic">
<a name="37f6aj0n4h5vj2s12iutdi2ah3">&nbsp;&nbsp;&nbsp;&nbsp;2.between 10 an 20;  值在10到20之间。例：查询英语分数在80-100之间的同学，select name,english from exam where english between 80 and 100;</a>
</h3>
<h3 class="topic">
<a name="28i6j0otolnscc3r16j5l1acrg">&nbsp;&nbsp;&nbsp;&nbsp;3.in(10,20,30);  值是10或20或30。例：查询数学分数为75,76,77的同学，select name, math from exam where math in(75,76,77);</a>
</h3>
<h3 class="topic">
<a name="1jta2ndisndk69sufu6nmdjvc7">&nbsp;&nbsp;&nbsp;&nbsp;4.like '张pattern';  pattern可以是%或者_，如果是%则表示任意多字符，例：张三丰，张飞，张abcd;如果是_则表示一个字符张_，张飞符合。例：查询所有姓张的学生的成绩， select * from exam where name like '张%';</a>
</h3>
<h3 class="topic">
<a name="246mensd2d1mm5qdsgoq4l621a">&nbsp;&nbsp;&nbsp;&nbsp;5.逻辑运算符。在多个条件直接可以使用逻辑运算符 and or not 。 例：查询数学分数&gt;70，语文&gt;80的同学。select name from exam where math&gt;70 and chinese &gt;80;   查询缺考数学的学生的姓名：select name from exam where math is null;</a>
</h3>
<h3 class="topic">
<a name="22jb2h262g8lov1ul8v1gc47k9">&nbsp;&nbsp;3.order by 指定排序的列，排序的列即可是表中的列名，也可以是select后面指定的别名。order by [列名|别名] [asc|desc|...]   asc升序，desc降序。</a>
</h3>
<h3 class="topic">
<a name="761q82hkiv790aj5f3vpgdpi2m">&nbsp;&nbsp;&nbsp;1.例：对总分排序，按照从高到低的顺序输出。select name, (ifnull(math,0)+ifnull(chinese,0)+ifnull(english,0)) 总成绩 from exam order by desc;</a>
</h3>
<h3 class="topic">
<a name="4jbprllcl45ar41s7vldo0rsip">&nbsp;&nbsp;4.聚合函数：技巧，先不管聚合函数要干什么，先把要求的内容查出来包上聚合函数即可。</a>
</h3>
<h3 class="topic">
<a name="05r90lva7h1c9kgjceliho61r8">&nbsp;&nbsp;&nbsp;1.count(列名);  例，统计一个班共有多少个学生，select count(*) from exam;</a>
</h3>
<h3 class="topic">
<a name="3jc9ioqfdm31g81s8uhdcrnjqb">&nbsp;&nbsp;&nbsp;2.sum(列名); //不会包含null   例，统计一个班级语文平均成绩，select sum(chinese)/count(*) from exam;</a>
</h3>
<h3 class="topic">
<a name="52bn72lftic6qepn22uond7f9h">&nbsp;&nbsp;&nbsp;3.avg(列名);  例，求一个班级的数学平均分，select avg(ifnull(math,0)) from exam;</a>
</h3>
<h3 class="topic">
<a name="799evgmipb6c3bm0rojc1uksmf">&nbsp;&nbsp;&nbsp;4.max/min   例，求班级最高分，select max(ifnull(math,0)+ifnull(chinese,0)+ifnull(english,0)) from exam;</a>
</h3>
<h3 class="topic">
<a name="41qgk6lttfnvaareqs9e96pk5q">&nbsp;&nbsp;5.group by子句：其后可以跟多个列名，也可以跟having子句对group by的结果进行筛选。</a>
</h3>
<h3 class="topic">
<a name="1o399h6tt0leod1l6s8hj2geno">&nbsp;&nbsp;&nbsp;1.例，对订单表中商品归类后，显示每一类商品的总价。select product, sum(price) from orders group by product; </a>
</h3>
<h3 class="topic">
<a name="5525ov3qil3nr6adv21ociq1dm">&nbsp;&nbsp;&nbsp;2.例，查询购买了几类商品，并且每类总价大于100的商品。select product, sum(price) from orders group by product having sum(price) &gt;100; </a>
</h3>
<h3 class="topic">
<a name="4u304661eveqncrke2u3lss0cr">&nbsp;&nbsp;&nbsp;3.having和where的差别： where语句在分组之前的筛选，having用在分组之后的筛选，having中可以使用聚合函数，where中就不行。使用where的地方可以使用having替换。</a>
</h3>
<h3 class="topic">
<a name="4obp8qdflrub5308jq41hekqdn">&nbsp;&nbsp;&nbsp;&nbsp;1.查询商品列表中除了橘子以外的商品，每一类商品的总价格大于500元的商品的名字。select product, sum(price) from orders where product&lt;&gt;'桔子&rsquo; group by having sum(price)&gt;500;</a>
</h3>
<h3 class="topic">
<a name="2oe84qcrfjco0t67u8p9bn0nr6">&nbsp;&nbsp;6.SQL关键字：select from where group by having order by  ;  MySQL在执行SQL语句时的顺序：from where select group by having order by ;</a>
</h3>
<h3 class="topic">
<a name="43ihldj67nmklfa7o37i00tk2i">&nbsp;9.数据完整性：是为了保证插入到数据中的数据是正确的，它防止了用户可能的输入错误。</a>
</h3>
<h3 class="topic">
<a name="5rbfre8min1slra7p397ruh8t6">&nbsp;&nbsp;1.实体（行）完整性：规定表的一行（即每一条记录）在表中是唯一的实体。</a>
</h3>
<h3 class="topic">
<a name="31urrvvcqachb027nh8posdf5f">&nbsp;&nbsp;&nbsp;1.通过定义主键约束来实现。主键：primary key（特点：不能为null，且唯一）</a>
</h3>
<h3 class="topic">
<a name="271ev1ntbbhoa713i8gpr5ku66">&nbsp;&nbsp;&nbsp;&nbsp;1.分为：逻辑主键：比如ID，不代表实际的业务意义，只用来唯一标识一条记录。（推荐）</a>
</h3>
<h3 class="topic">
<a name="452dpa7kve9qfh3i51orhsddhc">&nbsp;&nbsp;&nbsp;&nbsp;2.业务主键：比如username作为主键</a>
</h3>
<h3 class="topic">
<a name="7ven3ugm6nss8miioeu0jfprjt">&nbsp;&nbsp;&nbsp;2.主键的声明方式：</a>
</h3>
<h3 class="topic">
<a name="1pu255g96nq4ptb1n6omjq9sve">&nbsp;&nbsp;&nbsp;&nbsp;1.create table t1( id int primary, name varchar(100) );</a>
</h3>
<h3 class="topic">
<a name="3ltbaf3rip3bbd2l1f36doet3m">&nbsp;&nbsp;&nbsp;&nbsp;2.create table t2( id int,name varchar(100), primary(id) ); 此种方式可以定义联合主键</a>
</h3>
<h3 class="topic">
<a name="6b7uid571j1v35vt0leq3j47u2">&nbsp;&nbsp;&nbsp;&nbsp;3.推荐的方式。create table t3( id int, name varchar(100) ); alter table t3 add primary key(id);</a>
</h3>
<h3 class="topic">
<a name="7dkvb9f93b1o3u1r3ukrf6ufp1">&nbsp;&nbsp;&nbsp;&nbsp;4.自动增长的主键（只针对mysql，oracle没有）。create table t4( id int primary key auto_increment, name varchar(100) );</a>
</h3>
<h3 class="topic">
<a name="5egac3fj9p8tsk4b4mpb7pbo9g">&nbsp;&nbsp;2.域（列）完整性：指数据库表的列（即字段）必须符合某种特定数据类型或约束。</a>
</h3>
<h3 class="topic">
<a name="6200rbc2nnadmq9o0j8086gvq8">&nbsp;&nbsp;&nbsp;1.非空约束：not null     唯一约束：unique</a>
</h3>
<h3 class="topic">
<a name="0no28ig75dml2ljap91g29i6s6">&nbsp;&nbsp;&nbsp;2.例，create table t5( username varchar(100) not null unique, gender varchar(100) not null, phonenum unique );</a>
</h3>
<h3 class="topic">
<a name="1kusfem49po7n4een6jcq7i8qo">&nbsp;&nbsp;3.参照完整性：多表，外键约束。多表设计</a>
</h3>
<h3 class="topic">
<a name="5lc7ujlgtrt4m03jg2r0ojdjao">&nbsp;&nbsp;&nbsp;1.一对多：create table customers( id int,name varchar(100),address varchar(255), primary key(id) );    create table orders( id int primary key, order_num varchar(100), price float(8,2), status int, customer_id int, constrant(定义外键） customer_id_fk（约束名称：库中要唯一） foreign key(customer_id) References customers(id) );</a>
</h3>
<h3 class="topic">
<a name="4vg7ofn2077ss6jbk80g61f1g3">&nbsp;&nbsp;&nbsp;2.类和数据库表结构对应，对象和数据库中的记录对应。public class Customer{ private int id; private string name; private string address; private List&lt;Order&gt; orders=new ArrayList&lt;Order&gt;(); }  public class Order{ private int id; private string orderNum; private float price; private int status;  private Customer customer; }     </a>
</h3>
<h3 class="topic">
<a name="6js0mm888fq8tisk49kebqh3ri">&nbsp;&nbsp;&nbsp;3.多对多：create table teachers( id int primary key, name varchar(100), salary varchar(100) );   create table Students( id int primary key, name varchar(100),grade varchar(100) ); create table teacher_student( t_id int, s_id int, primary key(t_id,s_id),联合主键  constraint teacher_id_fk foreign key(t_id) references teachers(id), constraint student_id_fk foreign key(s_id) references students(id) );</a>
</h3>
<h3 class="topic">
<a name="7sthruc4fl8s919afu53dmu5t2">&nbsp;10.多表查询</a>
</h3>
<h2 class="topic">
<a name="2gj84shudnb9tekm6abbj49cso">十七、JDBC技术</a>
</h2>
<h2 class="topic">
<a name="1iuqk4k7ljh2uo1uob5la4e8kl">十八、事务</a>
</h2>
<h2 class="topic">
<a name="0iikfm57umv79folgab0iukaan">十九、数据库连接池</a>
</h2>
<h2 class="topic">
<a name="515evvmturp81abu24hm15hquc">二十、过滤器和监听器</a>
</h2>
<h2 class="topic">
<a name="45sq0maapnm98ctcjcpi0toc8r">二十一、文件上传</a>
</h2>
</body>
</html>
