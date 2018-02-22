##五、Android的assets资源使用##

- Android资源文件分类：

	Android资源文件大致可以分为两种：

	- 第一种是res目录下存放的可编译的资源文件：

    		这种资源文件系统会在R.java里面自动生成该资源文件的ID，所以访问这种资源文件比较简单，通过R.XXX.ID即可；

 	-	第二种是assets目录下存放的原生资源文件：

      		因为系统在编译的时候不会编译assets下的资源文件，所以我们不能通过R.XXX.ID的方式访问它们。
			那我么能不能通过该资源的绝对路径去访问它们呢？
			因为apk安装之后会放在/data/app/**.apk目录下，以apk形式存在，asset/res和被绑定在apk里，
			并不会解压到/data/data/YourApp目录下去，所以我们无法直接获取到assets的绝对路径，因为它们根本就没有。

	-	还好Android系统为我们提供了一个AssetManager工具类。

      	查看官方API可知，AssetManager提供对应用程序的原始资源文件进行访问；这个类提供了一个低级别的API，它允许你以简单的字节流的形式打开和读取和应用程序绑定在一起的原始资源文件。

		AssetManager类概述：

       		提供对应用程序的原始资源文件进行访问；这个类提供了一个低级别的API，它允许你以简单的字节流的形式打开和读取和应用程序绑定在一起的原始资源文件。

			通过getAssets()方法获取AssetManager对象。

		AssetManager类常用方法：

			Public Methods

			final String[]
	

			list(String path)

		返回指定路径下的所有文件及目录名。

			final InputStream
	

			open(String fileName)

		使用 ACCESS_STREAMING模式打开assets下的指定文件。.

			final InputStream
	

			open(String fileName, int accessMode)

使用显示的访问模式打开assets下的指定文件.

-	应用实例

	-	1.加载assets目录下的网页：

			//加载assets/win8_Demo/目录下的index.html网页
			webView.loadUrl("file:///android_asset/win8_Demo/index.html");

		说明：这种方式可以加载assets目录下的网页，并且与网页有关的css，js，图片等文件也会的加载。

	-	2.访问assets目录下的资源文件：

       		AssetManager.open(String filename)，返回的是一个InputSteam类型的字节流，这里的filename必须是文件比如（aa.txt；img/semll.jpg），而不能是文件夹。

	-	3.获取assets的文件及目录名：

			//获取assets目录下的所有文件及目录名，content（当前的上下文如Activity，Service等ContextWrapper的子类的都可以）
			String fileNames[] =context.getAssets().list(path);   
 
	-	4.将assets下的文件复制到SD卡：

			/** 
 			*  从assets目录中复制整个文件夹内容 
 			*  @param  context  Context 使用CopyFiles类的Activity
 			*  @param  oldPath  String  原文件路径  如：/aa 
 			*  @param  newPath  String  复制后路径  如：xx:/bb/cc 
 			*/ 
			public void copyFilesFassets(Context context,String oldPath,String newPath) {                    
         			try {
        				String fileNames[] = context.getAssets().list(oldPath);//获取assets目录下的所有文件及目录名
        				if (fileNames.length > 0) {//如果是目录
            				File file = new File(newPath);
            				file.mkdirs();//如果文件夹不存在，则递归
            				for (String fileName : fileNames) {
               					copyFilesFassets(context,oldPath + "/" + fileName,newPath+"/"+fileName);
           					}
        				} else {//如果是文件
           					InputStream is = context.getAssets().open(oldPath);
            				FileOutputStream fos = new FileOutputStream(new File(newPath));
            				byte[] buffer = new byte[1024];
            				int byteCount=0;               
            				while((byteCount=is.read(buffer))!=-1) {//循环从输入流读取 buffer字节        
                				fos.write(buffer, 0, byteCount);//将读取的输入流写入到输出流
            				}
            				fos.flush();//刷新缓冲区
            				is.close();
            				fos.close();
        				}
   				 	} catch (Exception e) {
        		
        			e.printStackTrace();
        			//如果捕捉到错误则通知UI线程
                   MainActivity.handler.sendEmptyMessage(COPY_FALSE);
    				}                           
			}
        

	-	5.使用assets目录下的图片资源：

			InputStream is=getAssets().open("wpics/0ZR424L-0.jpg");
			Bitmap bitmap=BitmapFactory.decodeStream(is);
			imgShow.setImageBitmap(bitmap);
	
	-	6.
	
			/**
	 		* 拷贝资产目录下的数据库文件
	 		* @param dbname  数据库文件的名称
	 		*/
			private void copyDB(final String dbname) {
				new Thread(){
					public void run() {
						try {
							File file = new File(getFilesDir(),dbname);
							if(file.exists()&&file.length()>0){
								Log.i("SplashActivity","数据库是存在的。无需拷贝");
							return ;
							}
							InputStream is = getAssets().open(dbname);
							FileOutputStream fos  = openFileOutput(dbname, MODE_PRIVATE);
							byte[] buffer = new byte[1024];
							int len = 0;
							while((len = is.read(buffer))!=-1){
								fos.write(buffer, 0, len);
							}
							is.close();
							fos.close();
						} catch (Exception e) {
							e.printStackTrace();
						}
					};
				}.start();
			}