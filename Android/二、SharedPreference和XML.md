##二、SharedPreference和XML##

- SharedPreference
	适合存储零散的数据，例如登录账户及密码等。

	使用方法：

		//取值
		mprefs=getSharedPreferences("config", MODE_PRIVATE);
		boolean configed=mprefs.getBoolean("configed", false);//false为默认值

		//存值
		SharedPreferences mpref;

		mpref=getSharedPreferences("config", MODE_PRIVATE);
		mpref.edit().putBoolean("configed", true).commit();

- XML:  XML文件可以用SAX、DOM或者pull解析
	
	- SAX是一个解析速度快且占用内存少的xml解析器，非常适合用于Android等移动设备。SAX解析XML文件采用的是事件驱动，也就是说，它并不需要解析完整个文档，在按内容顺序解析文档的过程中，SAX会判断当前读到的字符是否是合法XML语法中的某部分，如果符合就会触发事件。所谓事件就是一些方法的调用。
	
		SAX支持已内置到JDK1.5中，无需添加任何的jar文件。
		具体使用方法可参考自定义文档：数据的存储于访问：文件方式数据：用SAX对xml数据解析

	- DOM解析XML文件时，会将XML文件的所有内容以文档树方式存放在内存中，然后容许您使用DOM API遍历XML树、检索所需的数据。使用DOM操作XML的代码开起来比较直观，并且在编码方面比基于SAX的实现更加简单。但是DOM解析对内存消耗比较大，对于Android移动设备，内存资源比较宝贵，建议使用SAX解析。同样Android自带DOM解析。
	
	- Android系统本身使用到的各种xml文件，其内部也是采用Pull解析器进行解析的。下面以短信还原为例介绍Pull解析器。

	往XML中写数据：

		/**
	 	* 备份用户的短信
	 	* @param context
	 	* @param pd
	 	* @throws Exception
	 	*/
		public static void backupSms(Context context,BackUpCallBack callback) throws Exception{
		
			//如果短信非常多（成千上万条）的话，备份短信耗时较长，则不应当写到主线程
		
			ContentResolver resolver=context.getContentResolver();
			File file = new File(Environment.getExternalStorageDirectory(), "backup.xml");
			FileOutputStream fos=new FileOutputStream(file);

			//XML序列化器
			serializer = Xml.newSerializer();

			//把短信序列化到sd卡然后设置编码格式
			serializer.setOutput(fos, "utf-8");

			//第二个参数standalone表示当前的xml是否是独立文件 ture表示文件独立。
			serializer.startDocument("utf-8", true);

			//设置开始的节点 第一个参数是命名空间。第二个参数是节点的名字
			serializer.startTag(null, "smss");

			//设置smss节点上面的属性值 第二个参数是名字。第三个参数是值
			serializer.attribute(null, "size", String.valueOf(count));

			Uri uri=Uri.parse("content://sms/");
			Cursor cursor=resolver.query(uri, new String[]{"body","address","type","date"}, null, null, null);
		
			int max=cursor.getCount();
			//pd.setMax(max);
			int progress=0;
			callback.beforeBackUp(max);
		
			while(cursor.moveToNext()){
				String body=cursor.getString(0);
				String address=cursor.getString(1);
				String type=cursor.getString(2);
				String date=cursor.getString(3);
				
				//第三步
				serializer.startTag(null, "sms");
			
				//第四步
				serializer.startTag(null, "body");
				/**
				 * 加密：第一个参数表示加密种子(密钥)
				 *     第二个参数表示加密的内容
				 */
				serializer.text(Crypto.encrypt("123", cursor.getString(3)));

				//serializer.text(body);
				serializer.endTag(null, "body");
				
				serializer.startTag(null, "address");
				serializer.text(address);
				serializer.endTag(null, "address");
			
				serializer.startTag(null, "type");
				serializer.text(type);
				serializer.endTag(null, "type");
			
				serializer.startTag(null, "date");
				serializer.text(date);
				serializer.endTag(null, "date");
			
				serializer.endTag(null, "sms");
				progress++;
				//pd.setProgress(progress);
				callback.onBackUp(progress);
			}
			//第五步
			serializer.endTag(null, "smss");
			serializer.endDocument();
			fos.close();
		}

	从XML中读数据：

		/**
	 	* 还原短信
	 	* @param context
	 	* @param flag
	 	* @throws Exception 
	 	*/
		public static void restoreSms(Context context,boolean flag) throws 	Exception{
		
			Uri url=Uri.parse("content://sms/");
			if(flag){
				context.getContentResolver().delete(url, null, null);
			}
		
			//读取短信上的xml文件
			File file = new File(Environment.getExternalStorageDirectory(), "backup.xml");
			FileInputStream fis=new FileInputStream(file);

			//XML的pull解析器
			parser=Xml.newPullParser();

			parser.setInput(fis, "utf-8");

			int eventType=parser.getEventType();
			/**事件类型主要有五种
			 * START_DOCUMENT：xml头的事件类型
			 * END_DOCUMENT：xml尾的事件类型
			 * START_TAG：开始节点的事件类型
			 * END_TAG：结束节点的事件类型
			 * TEXT：文本节点的事件类型
			 * /
			 
			while(eventType != XmlPullParser.END_DOCUMENT){
				switch(eventType){
				case XmlPullParser.START_TAG:
					if("smss".equals(parser.getName())){
						smssinfo=new ArrayList<Message>();
					}else if("sms".equals(parser.getName())){
						message = new Message();
					}else if("body".equals(parser.getName())){
						String body=parser.nextText();
						message.setBody(body);
					}else if("address".equals(parser.getName())){
						String address=parser.nextText();
						message.setBody(address);
					}else if("type".equals(parser.getName())){
						String type=parser.nextText();
						message.setBody(type);
					}else if("date".equals(parser.getName())){
						String date=parser.nextText();
						message.setBody(date);
					}
					break;
				case XmlPullParser.END_TAG:
					if("sms".equals(parser.getName())){
						smssinfo.add(message);
					}else if("smss".equals(parser.getName())){
						message=null;
					}
					break;
				}
				eventType = parser.next();
			}
		
		
			//把短信插入到系统的短信应用中
			for(Message message:smssinfo){
			
				String body=message.getBody();
				String address=message.getAddress();
				String type=message.getType();
				String date=message.getDate();
			
				ContentValues values=new ContentValues();
				values.put("body", "wo shi duanxin");
				values.put("address", "138438000");
				values.put("type", "2");
				values.put("date", "13950717");
				context.getContentResolver().insert(url, values);
			}

		}