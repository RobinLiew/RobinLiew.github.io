# NPM工具的使用
### NPM基本介绍
- NPM是什么？NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题。NPM的中文文档https://www.npmjs.com.cn/getting-started/what-is-npm/上介绍说：
	它是世界上最大的软件注册表，每星期大约有 30 亿次的下载量，包含超过 600000 个 包（package） （即，代码模块）。来自各大洲的开源软件开发者使用 npm 互相分享和借鉴。包的结构使您能够轻松跟踪依赖项和版本。
	![这里写图片描述](http://img.blog.csdn.net/20180306162142661?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
     NPM就像一个超级管家一样，管理者各类js模块代码，并把js开发者联系起来。
 - NPM构成
	 npm 由三个独立的部分组成：

	    - 1网站  ：  网站是开发者查找包（package）、设置参数以及管理 npm 使用体验的主要途径。
	    - 2注册表（registry）  ：  注册表 是一个巨大的数据库，保存了每个包（package）的信息。
	    - 3命令行工具 (CLI)  ：  CLI 通过命令行或终端运行。开发者通过 CLI 与 npm 打交道。
	
	网站网址：https://www.npmjs.com
	![这里写图片描述](http://img.blog.csdn.net/20180306163016667?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
	你可以点进你感兴趣的项目，查看该项目如何使用等。

### NPM使用
- 因为我这里要用到Node.js，所以我下载Node.js后自带NPM。
![这里写图片描述](http://img.blog.csdn.net/20180306163538825?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 测试NPM是否安装成功 npm -v
![这里写图片描述](http://img.blog.csdn.net/20180306164102999?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如图显示NPM的版本号，表示NPM安装成功。
- 使用 npm 命令安装模块

	```
	npm install [-g] <Module Name> 
	
	没有参数-g表示本地安装，本地安装指的是：
		将安装包放在“运行npm命令时所在的目录” ，
		可以通过 require() 来引入本地安装的包。
		使用时，在代码中只需要通过 require('Module Name') 的方式就好，无需指定第三方包路径。 
	带有参数-g表示全局安装，全局安装是指：
		将安装包放在 /usr/local 下或者你 node 的安装目录，
		可以直接在命令行里使用
	Module Name指的是包名，即代码模块的名字。
	```

	我们使用 npm 命令安装cordova和ionic包,这里暂时不用管cordova和ionic包是什么，只需要这是相关的代码模块。


	```
	npm install -g cordova ionic  （网速较慢，下载时间长）
	或者
	npm install -g cnpm --registry=https://registry.npm.taobao.org （淘宝镜像）
	
	你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
	npm install -g cnpm --registry=https://registry.npm.taobao.org

	接下来就可以使用cnpm 命令来安装模块了其他：
	cnpm install -g ionic cordova 
	```
- 查看安装信息

	```
	查看所有全局安装的模块
	npm list -g
	
	查看某个模块的具体安装信息
	npm list <Module Name>
	```
- package.json文件
  因为我的window系统，并采用全局安装，因此安装的包位于：
  ![这里写图片描述](http://img.blog.csdn.net/20180306171313643?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
以ionic为例查看该包的package.json文件
![这里写图片描述](http://img.blog.csdn.net/20180306172440905?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- 卸载、更新、搜索模块

	```
	npm uninstall ionic
	卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：
	npm ls

	npm update ionic

	npm search ionic
	```
- 创建模块
创建模块请参考http://www.runoob.com/nodejs/nodejs-npm.html 。



 