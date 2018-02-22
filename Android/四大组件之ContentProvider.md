##四大组件之ContentProvider##

-	内容提供者
	
	ContentProvider
	四大组件之一
	
	内容提供者的作用：把私有数据暴露给其他应用，通常，是把私有数据库的数据暴露给其他应用

-	自定义内容提供者，继承ContentProvider类，重写增删改查方法，在方法中写增删改查数据库的代码，举例增方法

		@Override
		public Uri insert(Uri uri, ContentValues values) {
			db.insert("person", null, values);
			return uri;
		}
		
	在清单文件中定义内容提供者的标签，注意必须要有authorities属性，这是内容提供者的主机名，功能类似地址

		<provider android:name="com.robinliew.contentprovider.PersonProvider"
            	android:authorities="com.robinliew.person"
           	 	android:exported="true"
         	></provider>

	创建一个其他应用，访问自定义的内容提供者，实现对数据库的插入操作

		public void click(View v){
			//得到内容分解器对象
			ContentResolver cr = getContentResolver();
			ContentValues cv = new ContentValues();
			cv.put("name", "小方");
			cv.put("phone", 138856);
			cv.put("money", 3000);
			//url:内容提供者的主机名
			cr.insert(Uri.parse("content://com.robinliew.person"), cv);
		}

-	UriMatcher
	
	用于判断一条uri跟指定的多条uri中的哪条匹配
	
	添加匹配规则

		//指定多条uri
		um.addURI("com.robinliew.person", "person", PERSON_CODE);
		um.addURI("com.robinliew.person", "company", COMPANY_CODE);
		
		//#号可以代表任意数字
		um.addURI("com.robinliew.person", "person/#", QUERY_ONE_PERSON_CODE);

	通过Uri匹配器可以实现操作不同的表

		@Override
		public Uri insert(Uri uri, ContentValues values) {
			if(um.match(uri) == PERSON_CODE){
				db.insert("person", null, values);
			}
			else if(um.match(uri) == COMPANY_CODE){
				db.insert("company", null, values);
			}
			else{
				throw new IllegalArgumentException();
			}
			return uri;
		}
	如果路径中带有数字，把数字提取出来的api

		int id = (int) ContentUris.parseId(uri);

-	短信数据库

	只需要关注sms表，
	只需要关注4个字段

	* body：短信内容
	* address:短信的发件人或收件人号码（跟你聊天那哥们的号码）
	* date：短信时间
	* type：1为收到，2为发送
	
	读取系统短信，首先查询源码获得短信数据库内容提供者的主机名和路径，然后

		ContentResolver cr = getContentResolver();
		Cursor c = cr.query(Uri.parse("content://sms"), new String[]{"body", "date", "address", "type"}, null, null, null);
		while(c.moveToNext()){
			String body = c.getString(0);
			String date = c.getString(1);
			String address = c.getString(2);
			String type = c.getString(3);
			System.out.println(body+";" + date + ";" + address + ";" + type);
		}

	插入系统短信

		ContentResolver cr = getContentResolver();
		ContentValues cv = new ContentValues();
		cv.put("body", "您中大奖啦！！！");
		cv.put("address", 95555);
		cv.put("type", 1);
		cv.put("date", System.currentTimeMillis());
		cr.insert(Uri.parse("content://sms"), cv);
	插入查询系统短信需要注册权限

-	联系人数据库

	raw\_contacts表：
	* contact_id：联系人id
	data表：联系人的具体信息，一个信息占一行
	* data1：信息的具体内容
	* raw\_contact_id：联系人id，描述信息属于哪个联系人
	* mimetype_id：描述信息是属于什么类型
	mimetypes表：通过mimetype_id到该表查看具体类型

	读取联系人
	先查询raw\_contacts表拿到联系人id

		Cursor cursor = cr.query(Uri.parse("content://com.android.contacts/raw_contacts"), new String[]{"contact_id"}, null, null, null);
	然后拿着联系人id去data表查询属于该联系人的信息

		Cursor c = cr.query(Uri.parse("content://com.android.contacts/data"), new String[]{"data1", "mimetype"}, "raw_contact_id = ?", new String[]{contactId}, null);
	得到data1字段的值，就是联系人的信息，通过mimetype判断是什么类型的信息

		while(c.moveToNext()){
			String data1 = c.getString(0);
			String mimetype = c.getString(1);
			if("vnd.android.cursor.item/email_v2".equals(mimetype)){
				contact.setEmail(data1);
			}
			else if("vnd.android.cursor.item/name".equals(mimetype)){
				contact.setName(data1);
			}
			else if("vnd.android.cursor.item/phone_v2".equals(mimetype)){
				contact.setPhone(data1);
			}
		}
	插入联系人
	先查询raw\_contacts表，确定新的联系人的id应该是多少
	把确定的联系人id插入raw\_contacts表

		cv.put("contact_id", _id);
		cr.insert(Uri.parse("content://com.android.contacts/raw_contacts"), cv);
	在data表插入数据
		插3个字段：data1、mimetype、raw\_contact_id
		
			cv = new ContentValues();
			cv.put("data1", "赵六");
			cv.put("mimetype", "vnd.android.cursor.item/name");
			cv.put("raw_contact_id", _id);
			cr.insert(Uri.parse("content://com.android.contacts/data"), cv);
			
			cv = new ContentValues();
			cv.put("data1", "1596874");
			cv.put("mimetype", "vnd.android.cursor.item/phone_v2");
			cv.put("raw_contact_id", _id);
			cr.insert(Uri.parse("content://com.android.contacts/data"), cv);

-	内容观察者
	
	当数据库数据改变时，内容提供者会发出通知，在内容提供者的uri上注册一个内容观察者，就可以收到数据改变的通知

		cr.registerContentObserver(Uri.parse("content://sms"), true, new MyObserver(new Handler()));
		
		class MyObserver extends ContentObserver{

			public MyObserver(Handler handler) {
				super(handler);
				// TODO Auto-generated constructor stub
			}
	
			//内容观察者收到数据库发生改变的通知时，会调用此方法
			@Override
			public void onChange(boolean selfChange) {

			}
		
		}
		
		在内容提供者中发通知的代码

		ContentResolver cr = getContext().getContentResolver();
		//发出通知，所有注册在这个uri上的内容观察者都可以收到通知
		cr.notifyChange(uri, null);		