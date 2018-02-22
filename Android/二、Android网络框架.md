##二、Android网络框架##

-	HttpClient框架：由Apache组织开发的网络访问的开源框架，谷歌将其分装到了Android中
	-	发送get请求
		
			创建一个客户端对象

			HttpClient hc = new DefaultHttpClient();

			创建一个get请求对象
			String path="http://192.168.10.03/web/servlet/CheckLogin?name="+URLEncoder.encode(name)+"&pass="+pass;
			HttpGet hg = new HttpGet(path);

			发送get请求，建立连接，返回响应头对象

			HttpResponse hr = hc.execute(hg);

			获取状态行对象，获取状态码，如果为200则说明请求成功

			if(hr.getStatusLine().getStatusCode() == 200){
				//拿到服务器返回的输入流
				InputStream is = hr.getEntity().getContent();
				String text = Utils.getTextFromStream(is);
			}
	
	-	发送post请求

			//创建一个客户端对象
			HttpClient client = new DefaultHttpClient();
			//创建一个post请求对象
			String path="http://192.168.10.03/web/servlet/CheckLogin";
			HttpPost hp = new HttpPost(path);

			往post对象里放入要提交给服务器的数据
	
			//要提交的数据以键值对的形式存在BasicNameValuePair对象中
			List<NameValuePair> parameters = new ArrayList<NameValuePair>();
			NameValuePair bnvp = new BasicNameValuePair("name", name);
			NameValuePair bnvp2 = new BasicNameValuePair("pass", pass);
			parameters.add(bnvp);
			parameters.add(bnvp2);

			//创建实体对象，指定进行URL编码的码表
			UrlEncodedFormEntity entity = new UrlEncodedFormEntity(parameters, "utf-8");
			//为post请求设置实体
			hp.setEntity(entity);
			HttpResponse hr=hc.execute(hp);

			if((hr.getStatusLine().getStatusCode())==200){
				InputStream is=hr.getEntity().getContent();
				String text=Utils.getTextFormStream(is);
				Message msg=handler.obtainMessage();
				msg.obj=text;
				handler.sendMessage(msg);
			}

-	异步HttpClient框架：民间人士对HttpClient进行进一步的封装，在github上下载AsyncHttpClient，可以将其library文件夹下的代码直接拷贝到src下（直接复制com文件夹即可），也可将library作为库文件。

	-	发送get请求
	
			//创建异步的httpclient对象
			AsyncHttpClient ahc = new AsyncHttpClient();
			//发送get请求
			ahc.get(path, new MyHandler());
			注意AsyncHttpResponseHandler两个方法的调用时机

			class MyHandler extends AsyncHttpResponseHandler{

				//http请求成功，返回码为200，系统回调此方法
				@Override
				public void onSuccess(int statusCode, Header[] headers,
						//responseBody的内容就是服务器返回的数据
						byte[] responseBody) {
					Toast.makeText(MainActivity.this, new String(responseBody), 0).show();
				
				}
	
				//http请求失败，返回码不为200，系统回调此方法
				@Override
				public void onFailure(int statusCode, Header[] headers,
						byte[] responseBody, Throwable error) {
					Toast.makeText(MainActivity.this, "返回码不为200", 0).show();
				
				}
		
			}

	-	发送post请求

			使用RequestParams对象封装要携带的数据
 
			//创建异步httpclient对象
			AsyncHttpClient ahc = new AsyncHttpClient();
			//创建RequestParams封装要携带的数据
			RequestParams rp = new RequestParams();
			rp.add("name", name);
			rp.add("pass", pass);
			//发送post请求
			ahc.post(path, rp, new MyHandler());
			
			//MyHandler对象与上面get相同

-	xUtils框架的使用参考：RobinWiki/开源项目的使用/xutils框架
	
	HttpUtils本身就支持多线程断点续传，使用起来非常的方便

	创建HttpUtils对象

		HttpUtils http = new HttpUtils();
	下载文件
		
		http.download(url, //下载请求的网址
				target, //下载的数据保存路径和文件名
				true, //是否开启断点续传
				true, //如果服务器响应头中包含了文件名，那么下载完毕后自动重命名
				new RequestCallBack<File>() {//侦听下载状态
			
			//下载成功此方法调用
			@Override
			public void onSuccess(ResponseInfo<File> arg0) {
				tv.setText("下载成功" + arg0.result.getPath());
			}
			
			//下载失败此方法调用，比如文件已经下载、没有网络权限、文件访问不到，方法传入一个字符串参数告知失败原因
			@Override
			public void onFailure(HttpException arg0, String arg1) {
				tv.setText("下载失败" + arg1);
			}
			
			//在下载过程中不断的调用，用于刷新进度条
			@Override
			public void onLoading(long total, long current, boolean isUploading) {
				super.onLoading(total, current, isUploading);
				//设置进度条总长度
				pb.setMax((int) total);
				//设置进度条当前进度
				pb.setProgress((int) current);
				tv_progress.setText(current * 100 / total + "%");
			}
		});

-	json文件的解析
		
	-	可以使用Android自带的API解析json文件	

			简单的json文件
			{"versionName":"3.0",
			"versionCode":2,
			"description":"更新了什么，修复了什么",
			"downloadUrl":"http://www.robinliew.com"}

	
			InputStream in=conn.getInputStream();
			String result=StreamUtils.readFromStream(in);
						
						
			JSONObject jo=new JSONObject(result);
			mversionname=jo.getString("versionName");
			mversioncode=jo.getInt("versionCode");
			mdescription=jo.getString("description");
			mdownloadurl=jo.getString("downloadUrl");

			定义工具类
			public class StreamUtils {
				public static String readFromStream(InputStream in) throws IOException{
					ByteArrayOutputStream out=new ByteArrayOutputStream();
		
					byte[] buf=new byte[1024];
					int len=0;
					while((len=in.read(buf))!=-1){
						out.write(buf,0,len);
					}
				String result=out.toString();
				in.close();
				out.close();
				return result;
				}
			}	

	-	github上Gson框架进行解析
			
			Gson gson=new Gson();
			
			//NewsData类是分装json文件数据的JavaBean，但不需要设置set、get方法，
			//只需要保持该类中的字段名称与json文件中保存的键值对的键的名字完全相同即可
			NewsData data = gson.fromJson(result, NewsData.class);
			System.out.println("解析结果："+data.toString());
		
		