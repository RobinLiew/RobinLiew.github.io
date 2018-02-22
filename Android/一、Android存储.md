##一、Android存储##

- Android的存储

	内部存储空间
		
		RAM内存：运行内存，相当于电脑的内存

		ROM内存：存储内存，相当于电脑的硬盘
	
	外部存储空间

		SD卡：相当于电脑的移动硬盘
		2.2之前，sd卡路径：sdcard
		4.3之前，sd卡路径：mnt/sdcard
		4.3开始，sd卡路径：storage/sdcard

	所有存储设备，都会被划分成若干个区块，每个区块有固定的大小
	存储设备的总大小 = 区块大小 * 区块数量

	文件访问权限

		指的是谁能访问这个文件
		在Android中，每一个应用，都是一个独立的用户
		使用10个字母表示
		drwxrwxrwx
		第一个字母：
			d：表示文件夹
			-：表示文件
		第一组rwx：表示的是文件拥有者（owner）对文件的权限
			r：read，读
			w：write
			x：execute
		第二组rwx：表示的是跟文件拥有者属于同一用户组的用户（grouper）对文件的权限
		第三组rwx：表示的其他用户（other）对文件的权限

	含义	：

		运行内存是指手机运行程序时的内存，也叫RAM（简称运存）。
		机身内存相当于电脑的硬盘，厂家常直接称其为手机内存，也叫ROM（储存空间）。
		RAM：相当于电脑内存卡  ROM：相当于电脑C盘  SD：相当于电脑的其他盘

	区别：
	
		手机的“内存”通常指“运行内存”及“非运行内存”。
		RAM越大，手机能运行多个程序且流畅；手机内存越大，就像硬盘越大，能存放更多的数据。
		拥有更大的运行内存的话手机可以打开更多的程序，如果本身容量足够的话并不能提升多少运行程序的速度，只能说更大的运行内存能更好的保证手机的正常运行。
		手机的运行内存是指运行程序时存储或者暂时存储的地方，而CPU是用来计算的。

　　	RAM:  

		运行内存。RAM越大，手机可运行的APP应用程序越多，RAM越大手机运行速度越流畅（目前基本是2GB够用、3GB流畅、4GB用的更爽）。

　　	ROM:  

		储存空间。ROM越大，手机储存的文件数量越多，ROM的大小（16GB、32GB、64GB等）不影响手机运行速度。

　　	ROM一般包括：
		
		系统空间+用户安装程序空间+用户储存空间三个部分。

- 获取手机内存（ROM）、SD卡可用空间大小：

```
long romsize=getAvailSpace(Environment.getExternalStorageDirectory().getAbsolutePath());
		long sdsize=getAvailSpace(Environment.getDataDirectory().getAbsolutePath());

		String str_avail_sd = Formatter.formatFileSize(this, avail_sd);
		String str_avail_rom = Formatter.formatFileSize(this, avail_rom);
		//第一个参数是上下文，第二个是需要转换格式的long类型的文件大小。最终返回类似 22KB、52Bytes，22MB的字符串。

		/**
		 * 获取某个目录的可用空间
		 * @param path
	 	 * @return
	 	 */
		public long getAvailSpace(String path){
			//StatFs这个类是关键
			StatFs statfs=new StatFs(path);
			statfs.getBlockCount();//获取分区个数
			long size=statfs.getBlockSize();//获取分区的大小
			long count=statfs.getAvailableBlocks();//获取可用区块个数
		
			return size*count;
		}

```
		
-获取手机可用的剩余内存和总内存（RAM）：

参考安卓手机的Setting应用中显示运行进程占用的内存的写法，

```
/**
	 * 获取手机可用的剩余内存（动态内存，进程随时有可能产生或被杀死，内存不断地变化）
	 * @param context
	 * @return
	 */
	public static long getAvailMem(Context context){
		ActivityManager am=(ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
		MemoryInfo outinfo=new MemoryInfo();
		am.getMemoryInfo(outinfo);
		return outinfo.availMem;
	}
	
	/**
	 * 获取手机可用的总内存
	 * @param context
	 * @return long byte
	 */
	public static long getTotalMem(Context context){
	//		ActivityManager am=(ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
	//		MemoryInfo outinfo=new MemoryInfo();
	//		am.getMemoryInfo(outinfo);
	//		return outinfo.totalMem;
		//由于outinfo.totalMem这个api在低版本中不存在，为了保证兼容性，采用下面的代码获得手机内存
		
		File file=new File("/proc/meminfo");//手机内存的信息就保存在该文件夹下
		try {
			FileInputStream fis=new FileInputStream(file);
			BufferedReader br=new BufferedReader(new InputStreamReader(fis));
			String line=br.readLine();
			StringBuilder sb=new StringBuilder();
			for(char c:line.toCharArray()){
				if(c>='0'&&c<='9'){
					sb.append(c);
				}
			}
			return Long.parseLong(sb.toString())*1024;//返回以byte为单位的值
			//返回的这个数据需要调用Formatter.formatFileSize()方法格式化后显示

		} catch (Exception e) {
			e.printStackTrace();
		}
		return 0;
	}
```
	