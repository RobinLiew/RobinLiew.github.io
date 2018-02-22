##四大组件之Activity##

-	Activity生命周期
		
		oncreate:Activity对象创建完毕，但此时不可见
		onstart:Activity在屏幕可见，但是此时没有焦点
		onResume：Activity在屏幕可见，并且获得焦点
		onPause：Activity此时在屏幕依然可见，但是已经没有焦点
		onStop：Activity已经不可见了，但此时Activity的对象还在内存中
		onDestroy：Activity对象被销毁

	-	完整生命周期（entire lifetime）
	
			onCreate-->onStart-->onResume-->onPause-->onStop-->onDestory

	-	可视生命周期（visible lifetime）
			
			onStart-->onResume-->onPause-->onStop

	-	前台生命周期（foreground lifetime）
			
			onResume-->onPause

-	Activity任务栈

	-	应用运行过程中，内存中可能会打开多个Activity，那么所有打开的Activity都会被保存在Activity任务栈，一般来说一个应用打开就会创建一个任务栈。
	
	-	同一个程序，但不同的Activity是否可以放在不同的Task任务栈中？
	
	比方说在激活一个新的activity时候, 给intent设置flag
	Intent的flag添加FLAG_ACTIVITY_NEW_TASK singleinstance  单独的任务栈
   	这个被激活的activity就会在新的task栈里面…
	
		Intent intent = new Intent(A.this,B.class);
		intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
		startActivity(intent);

-	Activity的启动模式
	
	-	standard：标准模式,默认就是这个模式
			
														事件Task栈（右为栈顶组件）
			点开Email应用，进入收件箱（Activity A）					   A
			选中一封邮件，点击查看详情（Activity B）					 AB
			点击回复，开始写新邮件（Activity C）						  ABC
			写了几行字，点击选择联系人，进入选择联系人界面（Activity D）	 ABCD
			选择好了联系人，继续写邮件								  ABC
			写好邮件，发送完成，回到原始邮件							AB
			点击返回，回到收件箱										 A
			退出Email程序											null
	
	-	singleTop：如果目标Activity不在栈顶，那么就会启动一个新的Activity，如果已经在栈顶了，那么就不会再启动了
			
		作用： 为了防止JavaScript中的代码打开多个相同的Activity，用户点击返回时，体验相当不好。该模式可以很好的解决这个问题。
	
	-	singleTask：如果栈中没有该Activity，那么启动时就会创建一个该Activity，如果栈中已经有该Activity的实例存在了，那么在启动时，就会杀死在栈中处于该Activity上方的所有Activity全部杀死，那么此时该Activity就成为了栈顶Activity。
		
		作用：保证整个栈中只有一个该Activity的实例。当任务栈中存在耗时操作时，要保持任务栈中只有一个该Activity。例如，打开浏览器的Activity。
	
	-	singleInstance：设为此模式的Activity会有一个自己独立的任务栈，该Activity的实例只会创建一个，保存在独立的任务栈中
		
		作用：保证整个系统的内存中只有一个该Activity的实例。例如，10个应用都启动了拨号器，后台中会出现10个拨号器的Activity，这显然是不好的。singleInstance显然可以很好的解决该问题。
			
			例：
       		<activity
   				android:name="com.robinliew.activitymode.SecondActivity"
            	android:label="二" 
            	android:launchMode="singleTask">
			</activity>

-	横竖屏的切换
	
	Activity在横竖屏切换时会销毁重建，目的就是为了读取新的布局文件
	写死方向，不允许切换

		android:screenOrientation="portrait"
 		android:screenOrientation="landscape"
	配置Activity时添加以下属性，横竖屏切换时就不会销毁重建

		android:configChanges="orientation|keyboardHidden|screenSize"
		//使用代码锁死屏幕。标签和代码锁死屏幕，谁后执行，谁就生效。
        setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);

-	Activity的创建

	新创建的activity，必须在清单文件中做配置，否则系统找不到，在显示时会直接报错

		<activity android:name="com.robinliew.createactivity.SecondActivity"></activity>
	只要有以下代码，那么就是入口activity，就会生成快捷图标

			<intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

	如果Activity所在的包跟应用包名同名，那么可以省略不写

-	Activity的跳转	

	包括显示跳转和隐式跳转
	-	通过设置Activity的包名和类名实现跳转，称为显式意图。应用场景：一般启动同一个应用中的Activity
	
		跳转至同一项目下的另一个Activity，直接指定该Activity的字节码即可

			Intent intent = new Intent();
			intent.setClass(this, SecondActivity.class);
    		startActivity(intent);

			//实际编程过程中，习惯这样写
			startActivity(new Intent(this,SecondActivity.class));
		跳转至其他应用中的Activity，需要指定该应用的包名和该Activity的类名

			Intent intent = new Intent();
			//启动系统自带的拨号器应用
			intent.setClassName("com.android.dialer", "com.android.dialer.DialtactsActivity");
			startActivity(intent);

	-	通过指定动作实现跳转，称为隐式意图。应用场景：启动不同应用中的Activity
	
		隐式意图跳转至指定Activity

			Intent intent = new Intent();
			//启动系统自带的拨号器应用
    		intent.setAction(Intent.ACTION_DIAL);
    		startActivity(intent);
		要让一个Activity可以被隐式启动，需要在清单文件的activity节点中设置intent-filter子节点

			<intent-filter >
            	<action android:name="com.robinliew.second"/>
            	<data android:scheme="asd" android:mimeType="aa/bb"/>
            	<category android:name="android.intent.category.DEFAULT"/>
       		</intent-filter>
		-	action 指定动作（可以自定义，可以使用系统自带的）
			
			action节点的name是自己定义的，定义好之后，这个name的值就会成为这个activity动作，在隐式启动Activity时，意图中设置的action必须跟"com.robinliew.second"是完全匹配的
		
		-	data   指定数据（操作什么内容）
		
		-	category 类别 （默认类别，机顶盒，车载电脑）
		
		隐式意图启动Activity，需要为intent设置以上三个属性，且值必须与该Activity在清单文件中对三个属性的定义匹配
		intent-filter节点及其子节点都可以同时定义多个，隐式启动时只需与任意一个匹配即可

	-	URI 数据匹配

		一个 Intent 可以通过 URI 携带外部数据给目标组件。在 <intent-filter >节点中，通过 <data/>节点匹配外部数据。
		mimeType 属性指定携带外部数据的数据类型，scheme 指定协议，host、port、path 指定数据的位置、端口、和路径。如下：
 			
			<data android:mimeType="mimeType" android:scheme="scheme" 
 			android:host="host" android:port="port" android:path="path"/> 

		电话的uri   tel: 12345 
		   http://www.baidu.com
		
		自己定义的uri  itcast://cn.robinliew/person/10

		如果在 Intent Filter 中指定了这些属性，那么只有所有的属性都匹配成功时 URI 数据匹配才会成功。
		Category 类别匹配
		<intent-filter >节点中可以为组件定义一个 Category 类别列表，当 Intent 中包含这个列表的所有项目时 Category 类别匹配才会成功。
		默认是DEFAULT

	-	上述启动Activity的过程用到了Activity方法startActivity(...)
		
		我们可能会以为startActivity(...)方法是一个类方法，启动activity就是针对Activity子类调用该方法。实际并非如此。activity调用startActivity(...)方法时，调用请求实际发给了操作系统。

		准确的说，该方法调用请求是发送给操作系统的ActivityManager。ActivityManager负责创建Activity实例并调用onCreate(...)方法。

-	Activity间的数据传递
	
	-	intent携带数据的基本方法:

			//在Intent对象当中添加一个键值对
            intent.putExtra(key,value); //putExtra(String name,boolean value)                
            startActivity(intent);

			//取得从上一个Activity当中传递过来的Intent对象
            Intent intent = getIntent();
            //从Intent当中根据key取得value
            if (intent != null) {
                String value = intent.getStringExtra(key);
				//getBooleanExtra(String name,boolean defaultValue)
		
	-	利用Bundle对象传递数据
		
			Bundle bundle=new Bundle();  
            bundle.putString("title","我来啦");//把Activity 2传给下一个Activity  
            intent1.putExtras(bundle);  

			//获取bundle数据  
       		Bundle bundle=getIntent().getExtras();  
       		String text=bundle.getString("title");//根据key来获取 

	-	开启activity获取返回值
	
	从A界面打开B界面， B界面关闭的时候，返回一个数据给A界面
	步骤：
	1. 开启activity并且获取返回值
			
			Intent intent=new Intent(this, ContactActivity.class);
			startActivityForResult(intent, 0);//告诉系统，这个Activity销毁时会返回数据。
	2. 在新开启的界面里面实现设置数据的逻辑
	
			Intent data = new Intent();
			data.putExtra("phone", phone);
			//设置一个结果数据，数据会返回给调用者
			setResult(0, data);
			finish();//关闭掉当前的activity，才会返回数据

	3. 在开启者activity里面实现方法
			
			onActivityResult(int requestCode, int resultCode, Intent data) 
			通过data获取返回的数据
	4. 根据请求码和结果码确定业务逻辑
	

	-	返回结果的具体设置：
		
		-	实现子activity发送返回信息给父activity，有以下两种方法可供调用：
		
				public final void setResult(int resultCode)
				public final void setResult(int resultCode,Intent data)

			通常来说，参数resultcode可以是以下预定义常量中的任何一个：

				Activity.RESULT_OK;
				Activity.RESULT_CANCELED;

		-	在父类activity需要依据子activity的完成结果采取不同操作时，设置结果代码很有帮助。

			例如，假设子activity有一个ok按钮和cancel按钮，并且为每个按钮的单击动作分别设置了不同的结果代码。根据不同的结果代码，父activity会采取不同的操作。

		-	子activity可以不调用setResult(...)方法。如果不需要区分附加在intent上的结果或其他信息，可以让操作系统发送默认的结果代码。如果子activity是以调用startActivityForResult(...)方法启动的，结果代码则总是会返回给父activity。在没有调用setResult(...)方法的情况下，如果用户单击了后退按钮，父activity则会收到Activity.RESULT_CANCELED的结果代码。
