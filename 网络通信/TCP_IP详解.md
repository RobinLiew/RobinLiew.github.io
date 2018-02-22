注意：图片中的文字看起来有些小，不是很清楚，可以单击右键，选择点击“查看图像”，然后放大图像即可查看清晰的文字。

![这里写图片描述](http://img.blog.csdn.net/20171205200108454?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
<html>
<head>
<META http-equiv="Content-Type" content="text/html; charset=UTF-8">
<meta content="text/html; charset=utf-8" http-equiv="Content-Type">
<meta content="text/css" http-equiv="Content-Style-Type">
<title>TCP-IP详解卷1</title>
</head>
<body>
<h1 align="center" class="root">
<a name="3touudo3fl6uca57ncjpi00kdc">TCP-IP详解卷1</a>
</h1>
<div align="center" class="globalOverview">
<img src="TCP-IP%E8%AF%A6%E8%A7%A3%E5%8D%B71_files/images/TCP-IP%E8%AF%A6%E8%A7%A3%E5%8D%B71.jpg"></div>
<h2 class="topic">
<a name="18q1clk040hbs1q74rgqsv5ds4">第1章：概述</a>
</h2>
<h3 class="topic">
<a name="258lfpsrhcq1fq3qfu6fr1ulmf">&nbsp;1.TCP/IP分层：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="79lpnk3js14j2f9bd1u7ni7nj3">&nbsp;&nbsp;A.应用层（Telent：远程登录、FTP、e-mail等）；</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5orjh07dtuj6mtppsbgeqtf14l">&nbsp;&nbsp;B.运输层（TCP和UDP）；</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5lqr5tffdp595fc9inq9vchmj7">&nbsp;&nbsp;C.网络层（IP、ICMP、IGMP）；</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="023qdirtll8019se93e7gd7960">&nbsp;&nbsp;D.链路层（设备驱动程序及接口卡、ARP和RARP）</a>
</h3>
<h3 class="topic">
<a name="6njlemgs4eku99thghfn255f4b">&nbsp;2.端系统（例，两台主机）和中间系统（中间的路由器）。应用层和传输层使用端到端协议（End-to-end），网络层提供逐跳协议(Hop-by-hop)。</a>
</h3>
<h3 class="topic">
<a name="5rkp595gic1kv5jdsbdeoa2ih6">&nbsp;3.ICMP是IP协议的附属协议。IP层用它来与其他主机或路由器交换错误报文和其他重要信息。</a>
</h3>
<h3 class="topic">
<a name="4ajelbiq6e0h1f15c0lc0qcgvh">&nbsp;4.IGMP是Internet组管理协议。它用来把一个UDP数据报多播到多个主机。</a>
</h3>
<h3 class="topic">
<a name="297cnujg88uoj220pin624umqg">&nbsp;5.ARP（地址解析协议）和RARP（逆地址解析协议）是某些网络接口（如以太网和令牌环网）使用的特殊协议，用来装换IP层和网络层使用的地址。</a>
</h3>
<h3 class="topic">
<a name="2tte8m3rsgqc7iqk14bg2bdopg">&nbsp;6.互联网地址。A、B、C、D、E五类互联网地址。结构：头标志+网络号+主机号</a>
</h3>
<h3 class="topic">
<a name="1bp2ee61sg8o9tgb5m5dd906a2">&nbsp;7.三类IP地址：单播地址（目的为单个主机）、广播地址（目的端为给定网络上的所有主机）以及多播地址（目的端为同一组内的所有主机）</a>
</h3>
<h3 class="topic">
<a name="1ef2f994u5j036bsm7402bhhbc">&nbsp;8.域名系统（DNS）是一个分布的数据库，由它来提供IP地址和主机名之间的映射信息。</a>
</h3>
<h3 class="topic">
<a name="0kg7l686ah3vvla6p15mtc8rjd">&nbsp;9.封装：TCP报文段（或TCP段）-&gt; IP数据报 -&gt; 帧（Frame，通过以太网传输的比特流）</a>
</h3>
<h3 class="topic">
<a name="20isslam503h579pivar2ma5mn">&nbsp;10.分用：当目的主机收到一个以太网数据帧时，数据就开始从协议栈中由底向上升，同时去掉各层协议加上的报文首部。</a>
</h3>
<h3 class="topic">
<a name="26qskmg7v9p0pr4k8iui547ub0">&nbsp;11.客户-服务器模型：大部分网络应用程序在编写时都假设一端是客户，另一端是服务器，其目的是为了让服务器为客户提供一些特定的服务。这种服务分为两种类型：重复型或并发型。</a>
</h3>
<h3 class="topic">
<a name="1bsajrdjqhh5fc8biv97kmg078">&nbsp;12.端口号：任何TCP/IP实现所提供的服务都用知名的1~1023之间的端口号。大多数TCP/IP实现给临时端口分配1024-5000之间的端口号。大于5000的端口号是为其他服务器预留的。</a>
</h3>
<h3 class="topic">
<a name="6hhs0d90m6pi1j8vklj9up2pt0">&nbsp;13.应用编程接口。使用TCP/IP协议的应用程序通常采用两种应用编程接口（API）：socket和TLI（运输层接口，Transport Layer Interface）</a>
</h3>
<h2 class="topic">
<a name="72pg07q20lj25i44habvttv347">第2章：链路层</a>
</h2>
<h3 class="topic">
<a name="2u1jkqbe9p3umqm5155mlg013c">&nbsp;1.链路层的目的：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5mt0ok7afrhmp8r6pl8uu61o0d">&nbsp;&nbsp;A.为IP模块发送和接收IP数据报</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5r2oqu4sjo57ftel19lsfbrbp2">&nbsp;&nbsp;B.为ARP模块发送请求和接收ARP应答</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0tlsstnh4rudir2jcs3gi5pti0">&nbsp;&nbsp;C.为RARP发送RARP请求和接收RARP应答</a>
</h3>
<h3 class="topic">
<a name="7kquhsvnod5em9ob87nq384bu4">&nbsp;2.以太网和IEEE802封装（注意两种帧的格式封装的不同）。两种帧格式都采用48bit的目的地址和源地址（硬件地址）。ARP和RARP协议对32bit的IP地址和48bit的硬件地址进行映射。</a>
</h3>
<h3 class="topic">
<a name="1gg46p8rq7t1l4k9k4g5gt2ms0">&nbsp;3.SLIP：串行线路IP。开始结束用称作END的特殊字符表示，称作ESC的转义字符。</a>
</h3>
<h3 class="topic">
<a name="6sagvdgoqkcbnhea8o79q53vnq">&nbsp;4.PPP：点对点协议。注意PPP协议相对于SLIP协议的优点。</a>
</h3>
<h3 class="topic">
<a name="2ucp0i0jsroc3hp6dcr7er8kfu">&nbsp;5.环回接口：允许运行在同一台主机上的客户程序和服务程序通过TCP/IP进行通信。大多数系统把IP地址127.0.0.1分配给这个接口，并命名为localhost。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2eubaduo2oegceqvjbekaq0hh4">&nbsp;&nbsp;A.传给回环地址的任何数据均作为IP输入</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="14426o82uclfqv68s1uockmsro">&nbsp;&nbsp;B.传给广播地址或多播地址的数据报复制一份传给环回接口，然后送到以太网上。这是因为广播传送和多播传送的定义包含主机本身。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7n93h3696t1nr5if58mll7nivs">&nbsp;&nbsp;C.任何传给该主机IP地址的数据均送到环回接口。</a>
</h3>
<h3 class="topic">
<a name="4b7npdi6nrbqfofr7n1qlsi5t0">&nbsp;6.最大传输单元MTU和路径MTU。MTU是一个逻辑限制，目的是为了交互使用提供足够快的响应时间。</a>
</h3>
<h2 class="topic">
<a name="0hanjbjujj7jhmcu9vr6bhbe47">第3章：IP网际协议</a>
</h2>
<h3 class="topic">
<a name="6clusuq2mp8kb6o843seff1ra5">&nbsp;1.提供不可靠、无连接的数据报传送服务</a>
</h3>
<h3 class="topic">
<a name="0s8mnfdtapj55ghmeortb73f1p">&nbsp;2.不可靠：它不能保证IP数据报能成功地到达目的地</a>
</h3>
<h3 class="topic">
<a name="24emueb7q7h76ru6u78mitepfg">&nbsp;3.无连接：IP并不维护任何关于后续数据报的状态信息，每个数据报的处理是相互独立的。</a>
</h3>
<h3 class="topic">
<a name="5ssvonqonmlpfrdjvrs3rlkbmu">&nbsp;4.IP首部（普通IP首部长为20个字节）</a>
</h3>
<h3 class="topic">
<a name="0mbpi9u8rsdi0ri33u8j1r6ocq">&nbsp;&nbsp;1.big endian字节序。TCP/IP首部中所有的二进制整数在网络中传输时都要求以这种次序，因此它又称作网络字节序。其他形式存储二进制整数的机器，如little endian格式，则必须在传输数据之前把首部转换成网络字节序。</a>
</h3>
<h3 class="topic">
<a name="7e8n4lrpu6h3366qlml3jhdohn">&nbsp;&nbsp;2.注意IP数据报格式及首部中的各字段含义</a>
</h3>
<h3 class="topic">
<a name="758lr241kk7k5svo4ivtu8g1t0">&nbsp;&nbsp;&nbsp;1.服务类型（TOS）：有4bit的TOS分别代表：最小时延、最大吞吐量、最高可靠性和最小费用。大多数实现不使用TOS字段。</a>
</h3>
<h3 class="topic">
<a name="243o2bbscrrae630mfnjmsmivb">&nbsp;&nbsp;&nbsp;2.总长度字段是指整个IP数据报的长度。该字段长16bit，所以IP数据报最长可达65535字节（MTU）。尽管可以传递一个长达65535字节的IP数据报，但大多数链路层都会对它进行分片。</a>
</h3>
<h3 class="topic">
<a name="18ktti62ts147ecpj05s8jao6g">&nbsp;&nbsp;&nbsp;3.TTL（time-to-time）生存时间字段设置了数据报可以经过的最多路由数。它指定了数据报的生存时间。TTL的初始值由源主机设置（通常为32或64），一旦经过一个处理它的路由器，它的值就减1</a>
</h3>
<h3 class="topic">
<a name="7li0m1bciflbeqshap4kcbn21e">&nbsp;5.IP路由选择</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6pvv8k6a70ag63r25927vbrrj0">&nbsp;&nbsp;A.从概念上说，IP路由选择是简单的，特别是对于主来说。如果目的主机与源主机直接相连（如点对点链路）或者都在一个共享网络上（以太网或令牌环网），那么IP数据报就直接送到目的主机上。否则，主机把数据报发往一默认的路由器上，由路由器来转发该数据报。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7se1n83luq52rvdbsa5pj9rtm7">&nbsp;&nbsp;B.IP层在内存中有一个路由表。当收到一份数据报并进行发送时，它都要对该表搜索一次。当数据报来自某个网络接口时，IP首先检查目的IP地址是否为本机的IP地址之一或者IP广播地址。如果确实是这样，数据报就被送到由IP首部协议字段所指定的协议模块进行处理。如果数据报的目的不是这些地址，那么（1）如果IP层被设置为路由器的功能，那么就对数据报进行转发；否则（2）数据报被丢弃。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="19np4eqo595h99j422vpl248ol">&nbsp;&nbsp;C.路由表的每一项都包含下面这些信息：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5cq6adp988qdiv7gaqqimf4j02">&nbsp;&nbsp;&nbsp;a.目的IP地址。可以为完整的主机地址，也可以是一个网络地址，由该表目中的标志字段来指定。主机地址有一个非0的主机号，以指定某一特定的主机，而网络地址中的主机号为0，以指定网络中所有的主机。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3er738psdgjfrnt8n259e7qvlq">&nbsp;&nbsp;&nbsp;b.下一站路由器的IP地址，或者有直接连接的网络IP地址。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0srg0okr1u5eno3l922gac8ata">&nbsp;&nbsp;&nbsp;c.标志。其中一个标志指明目的IP地址是网络地址还是主机地址，另一个标志指明下一站路由器是否为真正的下一站路由器。还是一个直接相连接的接口。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3j6lum7uvaid7tlopfm19ueid7">&nbsp;&nbsp;&nbsp;d.为数据报的传输指定一个网络接口。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3tkfqhhphincrpbb52l4s2r90k">&nbsp;&nbsp;D.为一个网络指定一个路由器，而不必为每个主机指定一个路由器，这是IP路由选择机制的另一个基本特性。这样做可以极大地缩小路由表的规模。</a>
</h3>
<h3 class="topic">
<a name="0gf5qfl95rqmpshse4sgla6c91">&nbsp;6.子网寻址</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="1q02hiug38v1u2h9f6c0k0juja">&nbsp;&nbsp;A.不是把IP地址看成由单纯的网络号和一个主机号组成，而是把主机号再分成一个网络号和主机号。原因：A类和B类地址为主机号分配了太多的空间，事实上，一个网络中人们并不安排这么多的主机。例：B类地址，网络号（16位）+子网号（8位）+主机号（8位）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5ijfb8tstj2qihl4pmhk783v2g">&nbsp;&nbsp;B.子网对外部路由器来说隐藏了内部网络组织的细节，对子网内部的路由器是不透明的。</a>
</h3>
<h3 class="topic">
<a name="1llbt3k4fp69ddlfvgoj7g4bsk">&nbsp;7.子网掩码</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6s0oe3mt7fc3dt681r3d9ki1p0">&nbsp;&nbsp;A.子网掩码是一个32bit的值，其中值为1的比特留给网络号和子网号，为0的比特留给主机号。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3f8daiielfhnbvfftve54tp584">&nbsp;&nbsp;B.给定IP地址和子网掩码以后，主机就可以确定IP数据报的目的是：（1）本子网上的主机；（2）本网络中其他子网中的主机；（3）其他网络上的主机</a>
</h3>
<h3 class="topic">
<a name="54i6on5e99mjiqrbch63p8qb8c">&nbsp;8.ipconfig命令和netstat（-n参数打印出IP地址，而不是主机名字）命令</a>
</h3>
<h3 class="topic">
<a name="45ffr8n5ujq7vgfjd56l56qpkq">&nbsp;9.网络字节序。4个字节的32bit以下面的次序传输：首先是0~7bit，其次8~15bit，然后16bit~23bit，最后24~31bit。这种传输次序称作big endian字节序。由于TCP/IP首部中所有的二进制整数在网络中传输时都要求以这种次序，因此它又称作网络字节序。以其他形式存储二进制整数的机器，如little endian格式，则必须在传输数据之前把首部转换成网络字节序。</a>
</h3>
<h2 class="topic">
<a name="3f8vlfor3g629bb1p0agqeg21t">第4章：ARP地址解析协议</a>
</h2>
<h3 class="topic">
<a name="0e9ea7qojmah4p11emr3bjqvia">&nbsp;1.地址解析协议为这两种不同的地址形式提供映射：32bit的IP地址和数据链路层使用的任何类型的地址（一般为48bit）。</a>
</h3>
<h3 class="topic">
<a name="2o8cjocn87gutmic7ke8jpu9fl">&nbsp;2.ARP高速缓存。ARP高速运行的关键是由于每个主机上都有一个高速缓存，这个缓存存放了最近Internet地址到硬件地址之间的映射记录。高速缓存中每一项的生存时间一般为20分钟。</a>
</h3>
<h3 class="topic">
<a name="2b5erln6pjv6bmis8qq0pqaush">&nbsp;3.ARP的分组格式</a>
</h3>
<h2 class="topic">
<a name="1of5ku86k4v3tshp3um52t3kf3">第5章：RARP逆地址解析协议</a>
</h2>
<h2 class="topic">
<a name="04h9qj39vsn99f2avj3tst09b5">第6章：ICMP  Internet控制报文协议</a>
</h2>
<h3 class="topic">
<a name="7v6dcnpf0pv6l0rl2t4ovvjlsl">&nbsp;1.ICMP报文是在IP数据报内部被传输的。IP数据报（IP首部：ICMP报文）</a>
</h3>
<h3 class="topic">
<a name="2m3c79h2e6hm6o4vjfblp18kt7">&nbsp;2.具体格式参考TCP-IP详解卷1第6章</a>
</h3>
<h3 class="topic">
<a name="4h1vlbresi9e1t646guk4mc6ah">&nbsp;3.不会导致产生ICMP差错报文的情况（这些规则是为了防止过去容许ICMP差错报文对广播分组响应所带来的广播风暴）：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="23d3niokeqmfi5am0s0srvplo1">&nbsp;&nbsp;A.ICMP差错报文（但ICMP查询报文可能会产生ICMP差错报文）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="67cbeo6jpqo5jcfv0i9i8fg5e2">&nbsp;&nbsp;B.目的地址是广播地址或多播地址的IP数据报</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="27el63tjjcr90aiuudkholp0fh">&nbsp;&nbsp;C.作为链路层广播的数据报</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7n2vqe3o232o837hejupbfv7t9">&nbsp;&nbsp;D.不是IP分片的第一片</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5qjods7gh2jddetjvi87b118va">&nbsp;&nbsp;E.源地址不是单个主机的数据报。这就是说，源地址不能为零地址、环回地址、广播地址或多播地址。</a>
</h3>
<h3 class="topic">
<a name="39q8son7sk8gsho897naj6acu4">&nbsp;4.剩下的有时间再补充</a>
</h3>
<h2 class="topic">
<a name="3ucfgbv1qa6ittio896kuf2crd">第7章：UDP用户数据报协议</a>
</h2>
<h3 class="topic">
<a name="1e5ufa7i3ij0iih3r874qnggob">&nbsp;1.UDP首部各个字段。（16位源端口号：16位目的端口号：16位UDP长度：16位UDP校验和）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2qc7b9eag2030fnmkpu1do0058">&nbsp;&nbsp;A.端口号表示发送进程和接收进程</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="53bm4cti867r62a8180v38vup9">&nbsp;&nbsp;B.UDP端口号和TCP端口号是相互独立的</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="266vr31rusu8l1qo528q6m2flm">&nbsp;&nbsp;C.UDP长度字段指的是UDP首部和UDP数据的字节长度</a>
</h3>
<h3 class="topic">
<a name="5dhoic40700mfkfuuc57mfl87c">&nbsp;2.UDP校验和</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6nhjt1ematjof7o28cjkkb8eo2">&nbsp;&nbsp;A.UDP校验和覆盖UDP首部和UDP数据（作为对比，IP首部的校验和只覆盖IP首部，不覆盖IP数据报中的任何数据）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6r45jo3i0ij219d633oti76vnb">&nbsp;&nbsp;B.UDP数据报的长度可以为奇数字节，但检验和算法是把若干个16bit字相加。必要时在最后增加填充字节0，这只是为了检验和的计算。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7pnd5apjfv3gvjauak4rtbrnt9">&nbsp;&nbsp;C.UDP数据报和TCP段都包含一个12字节长的伪首部，它是为了计算校验和而设置的。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="1bncqn0447naugejig44ngao1m">&nbsp;&nbsp;D.校验算法流程：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2rnrq9ku7og6t7l9ivm8oj8jik">&nbsp;&nbsp;&nbsp;a.发送数据</a>
</h3>
<h3 class="topic">
<a name="4slgbvm1k8586a2hdtgvr0brh7">&nbsp;&nbsp;&nbsp;&nbsp;1.把校验和字段置为0；</a>
</h3>
<h3 class="topic">
<a name="28mn9ln2ne4vedm2nt0ld84dit">&nbsp;&nbsp;&nbsp;&nbsp;2.把需要校验的数据看成以16位为单位的数字组成，依次进行二进制反码求和；</a>
</h3>
<h3 class="topic">
<a name="56558dn581qip5ur26cbrhhb0d">&nbsp;&nbsp;&nbsp;&nbsp;3.把得到的结果存入校验和字段中；</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6g3t6p4tupjro68orom598kspj">&nbsp;&nbsp;&nbsp;b.接收数据</a>
</h3>
<h3 class="topic">
<a name="2v0sdvn1jrgp6b4j90nkhjvho6">&nbsp;&nbsp;&nbsp;&nbsp;1.把首部看成以16位为单位的数字组成，依次进行二进制反码求和，包括校验和字段；</a>
</h3>
<h3 class="topic">
<a name="0dbllmkjqlil9go4sbmhej87t9">&nbsp;&nbsp;&nbsp;&nbsp;2.检查计算出的校验和的结果是否为0；</a>
</h3>
<h3 class="topic">
<a name="0c63uirek6149lqpktgvifgbnq">&nbsp;&nbsp;&nbsp;&nbsp;3.如果等于0，说明被整除，校验和是正确。否则，校验和就是错误的，协议栈要抛弃这个数据包。</a>
</h3>
<h3 class="topic">
<a name="75ikrnfulec5hb3pr02a6pfn6a">&nbsp;3.IP分片</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0gu1nj03s6p6a5j24s7oapisji">&nbsp;&nbsp;A.物理网络层一般要限制每次发送数据帧的最大长度。任何时候IP层接收到一份要发送的IP数据报时，它要判断向本地哪个接口发送数据，并查询该接口获得其MTU。IP把MTU与数据报长度进行比较，如果需要则进行分片。分片可以发生在原始发送端主机上，也可以发生在中间路由器上。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="672l0b03g8p2pn4j043ptan9ql">&nbsp;&nbsp;B.一份IP数据报分片后，要在目的地才进行重新组装（这里的重新组装与其他网络协议不同，他们要求在下一站就进行重新组装，而不是最终目的地）。已经分片过的数据报有可能会再次进行分片（可能不止一次）。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="63uei3j4ro1meq05hvgse2c10r">&nbsp;&nbsp;C.当IP数据报被分片后，每一片都成为一个分组，具有自己的IP首部，并在选择路由时与其他分组独立。这样，当数据报的这些片到达目的端时有可能会失序，但是在IP首部中有足够的信息让接收端能正确组装这些数据报片。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3sj18skjk03n4a417hn8la0jv7">&nbsp;&nbsp;D.IP数据报是指IP层端到端的传输单元，分组是指IP层和链路层之间的传输单元，一个分组可以是一个完整的IP数据报，也可以是IP数据报的一个分片。</a>
</h3>
<h3 class="topic">
<a name="420ljfrdbln4ma2523i5vg68co">&nbsp;&nbsp;E.MTU发现机制：如果某个程序需要判断到达目的地端的路途中最小MTU是多少（是否分片需要通过与MTU的比较确定）</a>
</h3>
<h3 class="topic">
<a name="59vaiki8ovggoo6tkvus3j3nd2">&nbsp;4.最大UDP数据报长度：IP数据报的最大长度是65535字节，这是由IP首部16bit总长度字段所限制的。去除20字节的IP首部和8个字节的UDP首部，UDP数据报中用户数据的最长长度为65507字节。但大多数实现所提供的长度比这个最大值小。</a>
</h3>
<h3 class="topic">
<a name="0go3hvnu7ob3ans1jf58dho4a5">&nbsp;5.UDP输入队列：通常程序所使用的每个UDP端口都与一个有限大小的输入队列相联系。这意味着，来自不同客户的差不多同时到达的请求将由UDP自动排队。接收到的UDP数据报以其接收顺序交给应用程序。然而，排队溢出造成内核中UDP模块丢弃数据报的可能性是存在的。</a>
</h3>
<h2 class="topic">
<a name="4cs7r7km7b33bf5sca40odiju3">第8章：广播和多播</a>
</h2>
<h3 class="topic">
<a name="1d7mi6fa0o6r1bc0jqs91c6mnu">&nbsp;1.广播和多播仅应用于UDP，它们对需要将报文同时传往多个接收者的应用来说十分重要。TCP是一个面向连接的协议，它意味着分别运行于两主机（由IP地址确定）内的两进程（由端口号确定）间存在一条连接。</a>
</h3>
<h3 class="topic">
<a name="60m169vg8i3qear6d713qljpci">&nbsp;2.考虑包含多个主机的共享信道网络如以太网。每个以太网帧包含源主机和目的主机的以太网地址（48bit）。通常每个以太网仅发往单个目的主机，目的地址指明单个接收接口，因而称为单播。这种情况下，任意两个主机的通信不会干扰网内其他主机（可能引起争夺共享信道的情况除外）。</a>
</h3>
<h3 class="topic">
<a name="1doemlb8lkcorh1mirqiiiloep">&nbsp;3.有时一个主机要向网上的所有其他主机发送帧，这就是广播。多播处于单播和广播之间：帧仅传送给属于多播组的多个主机。</a>
</h3>
<h3 class="topic">
<a name="6bhhjit6qhn8pjapir7of90rh4">&nbsp;4.对于以太网，当地址中最高字节的最低位设置为1时表示该地址是一个多播地址，用十六进制可表示为01:00:00:00:00:00（以太网广播地址ff:ff:ff:ff:ff:ff可看做是以太网多播地址的特例）。</a>
</h3>
<h3 class="topic">
<a name="0vn438olj51qscfnq318h1qh5m">&nbsp;5.使用广播的问题在于它增加了对广播数据不感兴趣主机的处理负荷。使用多播，主机可以加入一个或多个多播组。这样，网卡将获悉该主机属于哪个多播组，然后仅接收主机所在的多播组的那些多播帧。</a>
</h3>
<h3 class="topic">
<a name="5k6n7jdipde1r1p277qaep11sc">&nbsp;6.四种IP广播地址</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3u8mrj4jpj7jhpinkuo1220kn2">&nbsp;&nbsp;A.受限的广播</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="285in2hikdec1gltvi7eu0ehf8">&nbsp;&nbsp;&nbsp;a.受限的广播地址是25.255.255.255  。该地址用于主机配置过程中IP数据报的目的地址，此时，主机可能还不知道它所在网络的网络掩码，甚至连它的IP地址也不知道。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7r5f1s92ft30eac1656an8s5jn">&nbsp;&nbsp;&nbsp;b.任何情况下，路由器都不转发目的地址为受限的广播地址的数据报，这样的数据报仅出现在本地网络中。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0551stkmfce8cfbgrcnm3iaafj">&nbsp;&nbsp;B.指向网络的广播</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="23tmck7g16k4pout0bbpfs2bit">&nbsp;&nbsp;&nbsp;a.指向网络的广播地址是主机号全1的地址。A类网络广播地址为netid.255.255.255，其中netid为A类网络的网络号。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="4iobhkka1vnhbrf2fk4f3esvhh">&nbsp;&nbsp;&nbsp;b.一个路由器必须转发指向网络的广播，但它也必须有一个不进行转发的选择。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="70oq0gvt95pe201fol9vjssadl">&nbsp;&nbsp;C.指向子网的广播</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="4cm9h1o7sftf2c4jhq05qstl6v">&nbsp;&nbsp;&nbsp;a.指向子网的广播地址为主机号为全1且有特定子网号的地址。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6fl511d6rildq5dednuikj24sv">&nbsp;&nbsp;&nbsp;b.通过子网掩码和IP地址确定指向子网的广播</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="1518ntorlof1p7v4kit2cfq4km">&nbsp;&nbsp;D.指向所有子网的广播</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="1akf3t1kq23a7mpsqfu69sp8e5">&nbsp;&nbsp;&nbsp;a.指向所有子网的广播也需要了解目的网络的子网掩码，以便与指向网络的广播地址区分开。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7231vilg8sco717j43dvgi1tll">&nbsp;&nbsp;&nbsp;b.指向所有子网的广播地址的子网号及主机号为全1。</a>
</h3>
<h3 class="topic">
<a name="73cr2ofomcs970nh4kp9iiufjm">&nbsp;7.多播</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6ig2eekj227gcs39cc5rb9kvdg">&nbsp;&nbsp;A.IP多播提供两类服务：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7cvg9jpoj93lhfqapbfbj3lhol">&nbsp;&nbsp;&nbsp;a.向多个目的地址传送数据。有许多向多个接收者传送信息的应用：例如交互式会议系统和向多个接收者发送邮件或新闻。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3h88f3hi1bmsjektd5fkfcuvcf">&nbsp;&nbsp;&nbsp;b.客户对服务器的请求。例如，无盘工作站需要确定启动引导服务器。目前，这项服务是通过广播来提供的，但使用多播可降低不提供这项服务主机的负担。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="24f351jl14g6or8je7p2b7lbdm">&nbsp;&nbsp;B.多播组地址（D类IP地址）。D类IP地址格式：1110+多播组ID（28位）。不像其他三类IP地址（A、B、C），分配的28bit均用作多播组号而不再表示其他。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2mnmmu3ibqmialsgube8bu2q6f">&nbsp;&nbsp;C.多播组地址为1110的最高4bit和多播组号。它们通常表示为点分十进制数，范围从224.0.0.0到239.255.255.255</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3v1gqi7aphfeebpup03hfpdkk5">&nbsp;&nbsp;D.能够接收发往一个特定多组播地址数据的主机集合称为主机组。一个主机组可以跨越多个网络。主机组中对主机的数量没有限制，同时不屑于某一主机组的主机可以向该组发送信息。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="4lqfp8huksl1lupt8aglmrbgqt">&nbsp;&nbsp;E.一些多播组地址被IANA确定为知名地址。例如，224.0.0.1代表&ldquo;该子网内的所有系统组&rdquo;，224.0.0.2代表&ldquo;该子网内的所有路由器组&rdquo;等。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="73cuk51k9g9b2a7fv3tr6ujnvd">&nbsp;&nbsp;F.多播组地址到以太网地址的转换</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="31s3nsiqihq5rv2d402cpjekfq">&nbsp;&nbsp;&nbsp;a.IANA拥有一个以太网地址块，即高位24bit为00:00:5e（十六进制表示），这意味着该地址所拥有的地址范围从00:00:5e:00:00:00到00:00:5e:ff:ff:ff。IANA将其中的一般分配为多播地址。为了指明一个多播地址，任何一个以太网地址的首字节必须是01，这意味着与IP多播相对应的以太网地址范围从01:00:5e:00:00:00到01:00:5e:7f:ff:ff</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0aj1elrkle011059sce3t0l6om">&nbsp;&nbsp;&nbsp;b.由于地址映射不是唯一的，因此需要其他的协议实现额外的数据报过滤。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0nl5fe99t180tgflfct2h43h2t">&nbsp;&nbsp;&nbsp;c.由于IP组播地址的高4bit是1110，标识了组播组，而低28bit中只有23bit被映射到组播MAC地址上，这样IP组播地址中就会有5bit没有使用，从而出现了32个IP组播地址映射到同一MAC地址上的结果</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3d5do93p1k5nkf82qh379qp584">&nbsp;&nbsp;&nbsp;d.网络中的主机通过发送IGMP报文相临近的路由器申请加入（或离开）组播组，交换机侦听用户与路由器之间的交互IGMP报文，跟踪组播信息及申请的端口。当交换机侦听到主机向路由器发出报文时，交换机用他来完成组播组的动态注册。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="10ga06t9hvc65oidc10842dads">&nbsp;&nbsp;&nbsp;e.IGMP侦听是组播约束机制。交换机侦听用户主机与路由器之间的交互IGMP报文，跟踪组播信息及其申请的端口。当交换机侦听到主机向路由器发送报告报文时，交换机便把该端口加入组播地址表中；当交换机侦听到主机发送离开的报文时，路由器会发送该端口的特定组查询报文，若还有其他主机需要该组播，则将回应报告报文，若路由器收不到任何主机的回应，交换机便把该端口从组播地址表中删除。</a>
</h3>
<h2 class="topic">
<a name="1cifrsi25aorpm5vgc8kbe5391">第9章：IGMP  Internet组管理协议</a>
</h2>
<h3 class="topic">
<a name="1gp1bveejnhslj0k6btqugcol6">&nbsp;1.IGMP用于支持主机和路由器进行多播。它让一个物理网络上的所有系统知道主机当前所在的多组播。多播路由器需要这些信息以便知道多播数据报应该向哪些接口转发。</a>
</h3>
<h3 class="topic">
<a name="31391dkk631bg565qgufj82irl">&nbsp;2.正如ICMP一样，IGMP也被当做IP层的一部分。IGMP报文通过IP数据报进行传输。不像我们已经见到的其他协议，IGMP有固定的报文长度，没有可选数据。封装在IP层中的格式：IP首部（20字节）+IGMP报文（8字节）</a>
</h3>
<h3 class="topic">
<a name="55mk9ktksi0d3ajjrbgi2f2b07">&nbsp;3.组播过程（一个典型的过程）：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0fei209iahe9o7pcih1n03jsci">&nbsp;&nbsp;A.IP主机的一个进程可随时加入和离开主机接口的一个组播组，该主机需要维护接口的一张表，该表包含了有哪些组以及这些组中的进程数量。此时主机需要发送一个IGMP报告。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="1vc05fakgo6b59c1jr58gok98o">&nbsp;&nbsp;B.路由器会定时发送IGMP查询报文，此时报文中组地址为0</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5e0bcpmcvut392j1f7mfsbb9bc">&nbsp;&nbsp;C.IP主机回应路由器的IGMP查询报文，对于一个主机，如果它加入了多个组，则需要为每个组返回一个IGMP报告。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="4gh0llgd6m4d4vek6851ro2gb6">&nbsp;&nbsp;D.路由器根据这些信息，会对每个接口维护一张表，表中说明了该接口的组。</a>
</h3>
<h2 class="topic">
<a name="3gf1ks4f1e6fu907n4lescsm5l">第10章：DNS域名系统</a>
</h2>
<h3 class="topic">
<a name="7o0tqegg33o51vrfpf7oas1sps">&nbsp;1.域名系统（DNS）是一种用于TCP/IP应用程序的分布式数据库，它提供主机名称和IP地址之间的转换及有关电子邮件的选路信息。</a>
</h3>
<h2 class="topic">
<a name="28mflcuhulk766i57f95ljgfc8">第11章：TCP传输控制协议</a>
</h2>
<h3 class="topic">
<a name="3bsj3tsjtp98coopdqa23hvas6">&nbsp;1.TCP通过下列方式提供可靠性：</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7lgc2565bg7aejhgat7k2ei36g">&nbsp;&nbsp;A.应用数据被分割成TCP认为最合适发送的数据块。这和UDP完全不同，应用程序产生的数据报长度将保持不变。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2lppnsmrhklqmvv2ilsn9sg1mm">&nbsp;&nbsp;B.当TCP发出一个段后，它将启动一个定时器，等待目的端确认收到这个报文段。如果不能及时收到一个确认，将重发这个报文段。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6maghu6dtvlj96gnq7sl3cqkpi">&nbsp;&nbsp;C.当TCP收到发自TCP连接另一端的数据，它将发送一个确认。这个确认不是立即发送，通常将推迟几分之一秒。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="65virmu0v6con0dghc6r9hasmi">&nbsp;&nbsp;D.TCP将保持它首部和数据的校验和。这是一个端到端的校验和，目的是检测数据在传输过程中的任何变化。如果收到段的校验和有差错，TCP将丢弃这个报文段和不确认收到此报文段（希望发送端超时并重发）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="35u25803le825tvmq07tk1ip1j">&nbsp;&nbsp;E.既然TCP报文段作为IP数据来传输，而IP数据报的到达可能会失序，因此TCP报文段的到达也可能会失序。如果必要，TCP将对收到的数据进行重新排序，将收到的数据以正确的顺序交给应用层。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="37hq1dk9mrk9g7abbf8l9j2e7s">&nbsp;&nbsp;F.既然IP数据报会发生重复，TCP的接收端必须丢弃重复的数据。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="1hkvli9pkmb1q2nltfbkqihqrg">&nbsp;&nbsp;G.TCP还能提供流量控制。TCP连接的每一方都有固定大小的缓冲空间。TCP的接收端只容许另一端发送接收端缓冲区所能接纳的数据。这将防止较快主机致使较慢主机的缓冲区溢出。</a>
</h3>
<h3 class="topic">
<a name="1fp12dvqff656tl78b0mgpqbae">&nbsp;2.TCP首部</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="632f20sm9sltfj48t8p4i7ogq4">&nbsp;&nbsp;A.16位源端口号+16位目的端口号。每个TCP段都包含源端和目的端的端口号，用于寻找发端和收段应用进程。这两个值加上IP首部中的源端IP地址和目的端IP地址唯一确定一个TCP连接。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="4kd0or9cgle6i6g26m7u4h8h5h">&nbsp;&nbsp;B.32位序号+32位确认序号。序号用来标识从TCP发端向TCP收端发送的数据字节流，它标识在这个报文段中的第一个数据字节。如果将字节流看做在两个应用程序间的单向流动，则TCP用序号对每个字节进行计数。序号是32bit的无符号数，序号到达2^32-1后又从0开始。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2sqgop8p62kdt46a11ccncfpok">&nbsp;&nbsp;C.4位首部长度+保留（6位）+6个标志比特</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3nccsgaeake7dro0827ilubjgr">&nbsp;&nbsp;&nbsp;a.URG紧急指针有效</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="12bdf2e4ske2mrp60nrl97ao0i">&nbsp;&nbsp;&nbsp;b.ACK确认序号有效</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="69t0ol47htc92mvu0pdh7j0jt9">&nbsp;&nbsp;&nbsp;c.PSH接收方应该尽快将这个报文段交给应用层</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="3vs4rv23b54qef8h5m32bkp14h">&nbsp;&nbsp;&nbsp;e.RST重建连接</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="32b1211t457ggm92fdh0lhotm0">&nbsp;&nbsp;&nbsp;f.SYN同步序号用来发起一个连接</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="30hfp9cfbfncd20at7vq7ift5b">&nbsp;&nbsp;&nbsp;g.FIN发端完成任务</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="17c0gknjmjojdlgc32ju06so7f">&nbsp;&nbsp;D.16位窗口大小。TCP的流量控制由连接的每一端通过声明的窗口大小来提供。窗口大小为字节数，起始于确认序号字段指明的值，这个值是接收端正期望接收的字节。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2s5ed7j2ebh2ckejnrq3fgu34b">&nbsp;&nbsp;E.16位校验和。校验和覆盖了整个的TCP报文段：TCP首部和TCP数据。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="0fqgbons96u2acep13h4s5i8v0">&nbsp;&nbsp;F.16位紧急指针。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="4jng0sklgvpandh62qb85b0kng">&nbsp;&nbsp;G.最常见的可选字段</a>
</h3>
<h3 class="topic">
<a name="79rmnl6ieas91rc8ve9fuo9g1s">&nbsp;3.建立连接协议（三次握手）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="2ijje41ukm7s93sgdmc1rjdq1u">&nbsp;&nbsp;A.请求端（通常称为客户）发送一个SYN段指明客户打算连接的服务器的端口。SYN=1，seq=J（客户将SYN置为1，发送序号为seq=J）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="6q309voesd66edne3l58gh8mgr">&nbsp;&nbsp;B.服务器由标志位SYN=1知道客户端要建立连接，服务端将自己的SYN和ACK置为1，应答序号置为ack=J+1，并将发送序号设为seq=K。SYN=1,ACK=1,ack=J+1,seq=K。</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="1uhfcdoea5f2guti2263iq7iuj">&nbsp;&nbsp;C.客户将检查ACK ?=1,ack ?=J+1，如果正确则将自己的ACK置为1，将应答序号设置为ack=K+1。服务端收到后会检查ack=K+1?，ACK=1?，如果正确则建立连接。ACK=1,ack=K+1</a>
</h3>
<h3 class="topic">
<a name="3oq0p4gqp5t0gekpt1d6gjakb0">&nbsp;4.连接终止协议（四次挥手）</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="26uet5ljm8llf7e4a19smui1o9">&nbsp;&nbsp;A.客户端发送一个FIN，用来关闭客户端到服务器的数据传输</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="7v52quqbp6cvmsjphf1tcaj2e0">&nbsp;&nbsp;B.服务端收到FIN后，发送一个ACK给客户端，确认序号为收到序号+1</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="37cua5tvrfe0ej9m9olrf18621">&nbsp;&nbsp;C.服务端发送一个FIN，用来关闭服务器到客户端的数据传输</a>
</h3>
<h3 class="topic">
&nbsp;&nbsp;<a name="5g6glh413sa93f5pc0eermjf15">&nbsp;&nbsp;D.客户端收到FIN后，确认序号置为发送序号+1</a>
</h3>
<h3 class="topic">
<a name="7tn0kjgr2d6rm5s45nb4mnr0i1">&nbsp;5.建立一个连接需要三次握手，而终止一个连接需要经过4次握手。这是由于TCP的半关闭造成的。既然一个TCP连接是全双工的（即数据能在两个方向上同时传输），因此每个方向必须单独关闭。</a>
</h3>
</body>
</html>
