##四大组件之BroadcastReceiver##

-	广播的概念：
		
		Android：系统在产生某个事件时发送广播，应用程序使用广播接收者接收这个广播，就知道系统产生了什么事件。
		
		Android系统在运行的过程中，会产生很多事件，比如开机、电量改变、收发短信、拨打电话、屏幕解锁
-	广播的两种类型
	
	-	无序广播：所有跟广播的intent匹配的广播接收者都可以收到该广播，并且是没有先后顺序（同时收到）
	
	-	有序广播：所有跟广播的intent匹配的广播接收者都可以收到该广播，但是会按照广播接收者的优先级来决定接收的先后顺序
	
	* 优先级的定义：-1000~1000
	
	* 最终接收者：所有广播接收者都接收到广播之后，它才接收，并且一定会接收
	
	* abortBroadCast：阻止其他接收者接收这条广播，类似拦截，只有有序广播可以被拦截
	
-	自定义广播的发送和接收

```
//自定义发送广播
		Intent intent=new Intent();
    	intent.setAction("com.liubin.sendzdy");
    	sendBroadcast(intent);


		//接收自定义广播
		//定义广播接收者
		public class ZDYReceiver extends BroadcastReceiver {

			@Override
			public void onReceive(Context context, Intent intent) {
		
				Toast.makeText(context, "收到自定义广播啦！", 0).show();
			}

		}
		//在清单文件中注册广播接收者
		<receiver android:name="com.robinliew.receiverzdy.ZDYReceiver" >
        	<intent-filter>
        	    <action android:name="com.robinliew.sendzdy"/>
        	</intent-filter>
        </receiver>"
```
		

-	定义广播接收者接收打电话广播（接收拨打电话的广播，修改广播内携带的电话号码）

```
public class CallReceiver extends BroadcastReceiver {

			//当广播接收者接收到广播时，此方法会调用
			@Override
			public void onReceive(Context context, Intent intent) {
				//拿到用户拨打的号码
				String number = getResultData();
				//修改广播内的号码
				setResultData("17951" + number);
			}
		}
	
		在清单文件中定义该广播接收者接收的广播类型

		<receiver android:name="com.robinliew.ipdialer.CallReceiver">
            <intent-filter >
                <action android:name="android.intent.action.NEW_OUTGOING_CALL"/>
            </intent-filter>
        </receiver>
		
		接收打电话广播需要权限

		<uses-permission android:name="android.permission.PROCESS_OUTGOING_CALLS"/>
```
		
		即使广播接收者的进程没有启动，当系统发送的广播可以被该接收者接收时，系统会自动启动该接收者所在的进程

-	系统收到短信时会产生一条广播，广播中包含了短信的号码和内容（短信拦截器）

		定义广播接收者接收短信广播
```
public void onReceive(Context context, Intent intent) {
			//拿到广播里携带的短信内容
			Bundle bundle = intent.getExtras();
			Object[] objects = (Object[]) bundle.get("pdus");//协议数据单元pdu
			for(Object ob : objects ){
				//通过object对象创建一个短信对象
				SmsMessage sms = SmsMessage.createFromPdu((byte[])ob);
				System.out.println(sms.getMessageBody());
				System.out.println(sms.getOriginatingAddress());
			}
		}
```
		

		系统创建广播时，把短信存放到一个数组，然后把数据以pdus为key存入bundle，再把bundle存入intent
		清单文件中配置广播接收者接收的广播类型，注意要设置优先级属性，要保证优先级高于短信应用，才可以实现拦截
```
<receiver android:name="com.robinliew.smslistener.SmsReceiver">
            <intent-filter android:priority="1000">
                <action android:name="android.provider.Telephony.SMS_RECEIVED"/>
            </intent-filter>
        </receiver>
		
		添加权限
		<uses-permission android:name="android.permission.RECEIVE_SMS"/>
```
		

	4.0以后广播接收者安装以后必须手动启动一次，否则不生效
	4.0以后广播接收者如果被手动关闭，就不会再启动了

-	清单文件中定义广播接收者接收的类型，监听SD卡常见的三种状态，所以广播接收者需要接收三种广播（监听SD卡状态）
		
		//清单文件的配置
		<receiver android:name="com.robinliew.sdcradlistener.SDCardReceiver">
            <intent-filter >
                <action android:name="android.intent.action.MEDIA_MOUNTED"/>
                <action android:name="android.intent.action.MEDIA_UNMOUNTED"/>
                <action android:name="android.intent.action.MEDIA_REMOVED"/>
                <data android:scheme="file"/>
            </intent-filter>
        </receiver>

	广播接收者的定义
```
public class SDCardReceiver extends BroadcastReceiver {
			@Override
			public void onReceive(Context context, Intent intent) {
				// 区分接收到的是哪个广播
				String action = intent.getAction();
					
				if(action.equals("android.intent.action.MEDIA_MOUNTED")){
					System.out.println("sd卡就绪");
				}
				else if(action.equals("android.intent.action.MEDIA_UNMOUNTED")){
					System.out.println("sd卡被移除");
				}
				else if(action.equals("android.intent.action.MEDIA_REMOVED")){
					System.out.println("sd卡被拔出");
				}
			}
		}

-	接收开机广播，在广播接收者中启动勒索的Activity
	
	清单文件中配置接收开机广播
```
这里写代码片
```
<receiver android:name="com.robinliew.BootReceiver">
            <intent-filter >
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
            </intent-filter>
        </receiver>
	权限

		<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```
		

	定义广播接收者
```
@Override
		public void onReceive(Context context, Intent intent) {
			//开机的时候就启动勒索软件
			Intent it = new Intent(context, MainActivity.class);		
			context.startActivity(it);
		}
	以上代码还不能启动MainActivity，因为广播接收者的启动，并不会创建任务栈，那么没有任务栈，就无法启动activity
	手动设置创建新任务栈的flag（注意广播接收者的启动不能创建任务栈，但可以在一个Activity中启动另一个Activity）

		it.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
```
		
-	监听应用的安装、卸载、更新
	原理：应用在安装卸载更新时，系统会发送广播，广播里会携带应用的包名
	清单文件定义广播接收者接收的类型，因为要监听应用的三个动作，所以需要接收三种广播
```
<receiver android:name="com.robinliew.app.AppReceiver">
            <intent-filter >
                <action android:name="android.intent.action.PACKAGE_ADDED"/>
                <action android:name="android.intent.action.PACKAGE_REPLACED"/>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package"/>
            </intent-filter>
        </receiver>
```
		
	广播接收者的定义
```
public void onReceive(Context context, Intent intent) {
			//区分接收到的是哪种广播
			String action = intent.getAction();
			//获取广播中包含的应用包名
			Uri uri = intent.getData();
			if(action.equals("android.intent.action.PACKAGE_ADDED")){
				System.out.println(uri + "被安装了");
			}
			else if(action.equals("android.intent.action.PACKAGE_REPLACED")){
				System.out.println(uri + "被更新了");
			}
			else if(action.equals("android.intent.action.PACKAGE_REMOVED")){
				System.out.println(uri + "被卸载了");
			}
		}
```
		
-	使用服务注册广播接收者
	
	Android四大组件都要在清单文件中注册
	广播接收者比较特殊，既可以在清单文件中注册，也可以直接使用代码注册
	
	有的广播接收者，必须代码注册
	* 电量改变
	* 屏幕锁屏和解锁
	
	这两种广播发送很频繁，但接收这两种广播的广播接收着不需要一直生效，不需要接收时，应解除注册。

	注册广播接收者
```
//创建广播接收者对象
		receiver = new ScreenOnOffReceiver();
		//通过IntentFilter对象指定广播接收者接收什么类型的广播
		IntentFilter filter = new IntentFilter();
		filter.addAction(Intent.ACTION_SCREEN_OFF);
		filter.addAction(Intent.ACTION_SCREEN_ON);
		
		//注册广播接收者
		registerReceiver(receiver, filter);
	解除注册广播接收者

		unregisterReceiver(receiver);
	//解除注册之后，广播接收者将失去作用
```
		