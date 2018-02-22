##一、Android网络编程基本知识##

-	网络请求较为耗时，写在主线程中容易导致主线程阻塞
	UI停止刷新，应用无法响应用户操作
	耗时操作不应该在主线程进行
		
		ANR
			* application not responding
			* 应用无响应异常
			* 主线程阻塞时间过长，就会抛出ANR

	主线程又称UI线程，因为只有在主线程中，才能刷新UI

-	消息队列机制
	* 主线程创建时，系统会同时创建消息队列对象（MessageQueue）和消息轮询器对象（Looper）
	* 轮询器的作用，就是不停的检测消息队列中是否有消息（Message）
	* 消息队列一旦有消息，轮询器会把消息对象传给消息处理器（Handler），处理器会调用handleMessage方法来处理这条消息，handleMessage方法运行在主线程中，所以可以刷新ui
	* 总结：只要消息队列有消息，handleMessage方法就会调用
	* 子线程如果需要刷新ui，只需要往消息队列中发一条消息，触发handleMessage方法即可
	* 子线程使用处理器对象的sendMessage方法发送消息
	
			在子线程中往消息队列里发消息

			//创建消息对象
			Message msg = new Message();
    		//消息的obj属性可以赋值任何对象，通过这个属性可以携带数据
			msg.obj = bm;
    		//what属性相当于一个标签，用于区分出不同的消息，从而运行不同的代码
			msg.what = 1;
    		//发送消息
    		handler.sendMessage(msg);

			//消息队列
			Handler handler = new Handler(){
			//主线程中有一个消息轮询器looper，不断检测消息队列中是否有新消息，如果发现有新消息，自动调用此方法，注意此方法是在主线程中运行的
				public void handleMessage(android.os.Message msg) {
		
				}
			};

			通过switch语句区分不同的消息

			public void handleMessage(android.os.Message msg) {
				switch (msg.what) {
				//如果是1，说明属于请求成功的消息
				case 1:
					ImageView iv = (ImageView) findViewById(R.id.iv);
					Bitmap bm = (Bitmap) msg.obj;
					iv.setImageBitmap(bm);
					break;
				case 2:
					Toast.makeText(MainActivity.this, "请求失败", 0).show();
					break;
				}		
			}

-	三次握手基本介绍

	-	在TCP/IP协议中，TCP协议提供可靠的连接服务，采用三次握手建立一个连接。

			第一次握手：建立连接时，客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；SYN：同步序列编号(Synchronize Sequence Numbers)。

			第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

			第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

			完成三次握手，客户端与服务器开始传送数据，

		SYN（synchronous）是TCP/IP建立连接时使用的握手信号
		TCP是传输控制协议。
		syn是该协议中的一个标志位。如果该位被置为1，则表示这个报文是一个请求建立连接的报文。
		ack也是该协议的一个标志位。如果该位被置为1，则表示这个报文是一个用于确认的报文。

-	Android中网络连接的基本操作

	-	我们以网络图片查看器为例介绍利用HttpURLConnection访问网络资源
	
			URL url = new URL(address);
			//获取连接对象，并没有建立连接
			HttpURLConnection conn = (HttpURLConnection) url.openConnection();
			//设置连接和读取超时
			conn.setConnectTimeout(5000);
			conn.setReadTimeout(5000);
			//设置请求方法，注意必须大写
			conn.setRequestMethod("GET");
			//建立连接，发送get请求
			//conn.connect();
			//建立连接，然后获取响应吗，200说明请求成功
			conn.getResponseCode();
			服务器的图片是以流的形式返回给浏览器的 

			//拿到服务器返回的输入流
			InputStream is = conn.getInputStream();
			//把流里的数据读取出来，并构造成图片
			Bitmap bm = BitmapFactory.decodeStream(is);
			把图片设置为ImageView的显示内容

			ImageView iv = (ImageView) findViewById(R.id.iv);
   			iv.setImageBitmap(bm);
			添加权限

	-	加入缓存图片的功能
			
			final String path="http://127.0.0.1:8080/dd.jpg";
    		final File file=new File(getCacheDir(),getFileName(path));
			
			//每次发送请求前检测一下在缓存中是否存在同名图片，如果存在，则读取缓存
    		if(file.exists()){
    			Bitmap bm=BitmapFactory.decodeFile(file.getAbsolutePath());
    			iv.setImageBitmap(bm);
    		}else{
				//开启子线程，进行网络图片资源的访问
				Thread thread=new Thread(){
        			public void run(){
        			
        				try {
							//利用HttpURLConnection访问网络资源的基本步骤
        					URL url=new URL(path);
        					HttpURLConnection conn=(HttpURLConnection) url.openConnection();
        					conn.setRequestMethod("GET");
        					conn.setConnectTimeout(2000);
        					conn.connect();

        					if(conn.getResponseCode()==200){
        						InputStream in=conn.getInputStream();
        					
        						byte[] buf=new byte[1024];
        			
        						FileOutputStream fos=new FileOutputStream(file);
        						int len=0;
        						while((len=in.read(buf))!=-1){
        						fos.write(buf);
        					}
        					//Bitmap bm=BitmapFactory.decodeStream(in);
        					Bitmap bm=BitmapFactory.decodeFile(file.getAbsolutePath());
        					
        					//利用handler消息机制给UI线程发送消息
        					Message msg=new Message();
        					msg.obj=bm;
        					msg.what=0;
        					handler.sendMessage(msg);
						}
					}
				}

	-	Html源文件查看器
	
			发送GET请求：

			URL url = new URL(path);
			//获取连接对象
			HttpURLConnection conn = (HttpURLConnection) url.openConnection();
			//设置连接属性
			conn.setRequestMethod("GET");
			conn.setConnectTimeout(5000);
			conn.setReadTimeout(5000);
			//建立连接，获取响应吗
			if(conn.getResponseCode() == 200){
				
			}
			获取服务器返回的流，从流中把html源码读取出来

			byte[] b = new byte[1024];
			int len = 0;
			ByteArrayOutputStream bos = new ByteArrayOutputStream();
			while((len = is.read(b)) != -1){
				//把读到的字节先写入字节数组输出流中存起来
				bos.write(b, 0, len);
			}
			//把字节数组输出流中的内容转换成字符串
			//默认使用utf-8
			text = new String(bos.toByteArray());
			乱码的处理
			乱码的出现是因为服务器和客户端码表不一致导致
		
			//手动指定码表
			text = new String(bos.toByteArray(), "gb2312");

-	Android提交数据
	-	GET方式提交数据
			
			get方式提交的数据是直接拼接在url的末尾

			final String path = "http://192.168.1.104/Web/servlet/CheckLogin?name=" + name + "&pass=" + pass;

			发送get请求，代码和之前一样

			URL url = new URL(path);
			HttpURLConnection conn = (HttpURLConnection) url.openConnection();
			conn.setRequestMethod("GET");
			conn.setReadTimeout(5000);
			conn.setConnectTimeout(5000);
			if(conn.getResponseCode() == 200){

			}
			浏览器在发送请求携带数据时会对数据进行URL编码，我们写代码时也需要为中文进行URL编码

			String path = "http://192.168.1.104/Web/servlet/CheckLogin?name=" + URLEncoder.encode(name) + "&pass=" + pass;

	-	POST方式提交数据
			
			post提交数据是用流写给服务器的

			协议头中多了两个属性
			Content-Type: application/x-www-form-urlencoded，描述提交的数据的mimetype
			Content-Length: 32，描述提交的数据的长度

			//给请求头添加post多出来的两个属性
			String data = "name=" + URLEncoder.encode(name) + "&pass=" + pass;
			conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
			conn.setRequestProperty("Content-Length", data.length() + "");

			设置允许打开post请求的流

			conn.setDoOutput(true);

			获取连接对象的输出流，往流里写要提交给服务器的数据

			OutputStream os = conn.getOutputStream();
			os.write(data.getBytes());
