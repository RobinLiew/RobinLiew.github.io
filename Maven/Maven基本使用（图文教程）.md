# Maven基本使用

## 一、安装
- 先去官网下载maven（http://maven.apache.org/download.cgi）
- 下载下来解压后如下所示
![这里写图片描述](http://img.blog.csdn.net/20180223133352895?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 配置环境变量
![这里写图片描述](http://img.blog.csdn.net/20180223133732674?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180223133741955?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 查看配置成功与否
![这里写图片描述](http://img.blog.csdn.net/20180223134057395?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- maven配置文件
![这里写图片描述](http://img.blog.csdn.net/20180223134253567?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- maven项目目录结构
![这里写图片描述](http://img.blog.csdn.net/2018022313551548?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 使用myeclipse构建maven项目
![这里写图片描述](http://img.blog.csdn.net/20180223140134174?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- pom.xml文件
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <parent>
    <groupId>com.robinliew.foo</groupId>
    <artifactId>foo-war</artifactId>
    <version>0.0.1</version>
  </parent>
  
  <!-- 
	maven 的所有构件均通过坐标进行组织和管理。maven 的坐标通过 5 个元素进行定义，其中 groupId、artifactId、version 是必须的，packaging 是可选的（默认为jar），classifier 是不能直接定义的。
	
	groupId组名，主项目标识  定义当前 Maven 项目所属的实际项目，跟 Java 包名类似
	artifactId - 工程名，子项目（模块）标识  定义当前 Maven 项目的一个模块，默认情况下，Maven 生成的构件，其文件名会以 artifactId 开头
	packaging - 打包方式  定义项目打包方式，如 jar，war，pom，zip ……，默认为 jar。
	version - 版本
	name - 项目描述名
	classifier：定义项目的附属构件。classifier 不能直接定义，通常由附加的插件帮助生成
 -->
  <groupId>com.robinliew.mavendemo</groupId>
  <version>0.0.1</version>
  <artifactId>com.robinliew.mavendemo</artifactId>
  <packaging>war</packaging>
  <name>mavendemo</name>
  <description>maven快速入门</description>
  <!-- 配置依赖的jar包 ->
  <dependencies>
	<dependency>
	        <groupId>junit</groupId>
		<artifactId>junit</artifactId>
	        <version>4.11</version>
	</dependency>
</dependencies>
  
</project>

```
- 依赖详解
![这里写图片描述](http://img.blog.csdn.net/20180224140752751?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

	- 其中依赖范围有如下几种
		- compile：编译依赖范围，默认值。此选项对编译、测试、运行三种 classpath 都有效，如 hibernate-core-3.6.5.Final.jar，表明在编译、测试、运行的时候都需要该依赖
		-  test：测试依赖范围。只对测试有效，表明只在测试的时候需要，在编译和运行时将无法使用该类依赖，如 junit；
		- provided：已提供依赖范围。编译和测试有效，运行无效。如 servlet-api ，在项目运行时，tomcat 等容器已经提供，无需 Maven 重复引入；
		- runtime：运行时依赖范围。测试和运行有效，编译无效。如 jdbc 驱动实现，编译时只需接口，测试或运行时才需要具体的 jdbc 驱动实现；
		- system：系统依赖范围。和 provided 依赖范围一致，需要通过 <systemPath> 显示指定，且可以引用环境变量；
		- import：导入依赖范围。使用该选项，通常需要 <type>pom</type>，将目标 pom 的 dependencyManagement 配置导入合并到当前 pom 的  dependencyManagement 元素。
	- 依赖传递和以来范围
	- 依赖冲突
	- 依赖排除
	
	
- 创建一个maven工程
![这里写图片描述](http://img.blog.csdn.net/20180224085801756?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
默认配置，点击两次next后
![这里写图片描述](http://img.blog.csdn.net/20180224085827841?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如果没有显示上面的archetype骨架列表，则需要我们先进行配置，点击configure如下
![这里写图片描述](http://img.blog.csdn.net/20180224090020611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
远程配置是通过访问http://repo1.maven.org/maven2/archetype-catalog.xml地址进行配置的，由于该文件较大，且访问外国网址速度很慢，可能需要等好一会，下面介绍另一种本地配置方式
![这里写图片描述](http://img.blog.csdn.net/20180224090354437?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这种方式需要提前下载配置文件通。过浏览器查看http://repo1.maven.org/maven2/archetype-catalog.xml页源码（注意：因为文件较大，网速较慢，请多等一会），复制到本地，命名为archetype-catalog.xml。
如果你的myeclipse出现卡顿现象，查看是否报GC overhead limit exceeded的错误，如果是可以通过下面的方式解决
![这里写图片描述](http://img.blog.csdn.net/20180224090938377?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180224090948196?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
创建好的maven工程如下所示
![这里写图片描述](http://img.blog.csdn.net/20180224091213741?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
可以看到默认创建的目录结构只有src/main/resources，缺少 src/main/java和src/test/java 。点击该项目后，右击，选择build path-->configure build path。
![这里写图片描述](http://img.blog.csdn.net/20180224092832895?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180224092729186?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180224092851256?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
设置完后我们可以查看maven输出目录
![这里写图片描述](http://img.blog.csdn.net/20180224093545328?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当然这是可以自己点击edit修改的，没有特殊需求的情况下一般不建议修改。
![这里写图片描述](http://img.blog.csdn.net/20180224092903810?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
此时maven项目的目录结构完整了。

设置 Project Facets,设置部署打包结构,删除测试相关目录
![这里写图片描述](http://img.blog.csdn.net/20180224094407254?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180224094459592?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
此时创建一个maven项目完成了。
- 构建及项目部署
右键 pom.xml - Run As - Maven -install
![这里写图片描述](http://img.blog.csdn.net/20180224131344968?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180224131354536?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 二、maven常用的命令
- 首先通过cmd进入要操作的项目目录
 ![这里写图片描述](http://img.blog.csdn.net/20180224132326163?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 - 常用的几种命令
	 - mvn compile  编译项目源代码
	 - mvn clean  删除 target 目录
	 - mvn test  运行测试（运行src/test/java中的测试代码）
	 - mvn clean package（组合命令）  Maven 自动帮我们完成项目的编译、测试、打包
	 - mvn install  效果跟mvn clean package命令一样，且项目被打包发布到了 maven 的仓库，以后其他项目需要依赖到这个项目，就可以通过在 pom.xml 文件中添加依赖来引用。
	 - mvn archetype:generate  自动创建Maven目录结构
		 - 下面是mvn archetype:generate 的使用例子
		 ![这里写图片描述](http://img.blog.csdn.net/20180224135119932?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 
		 默认回车即可，输入groupId等，回车
		 ![这里写图片描述](http://img.blog.csdn.net/20180224135131835?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 三、仓库
- maven使用一个仓库，通过声明依赖关系可以引用各个构件，各构件通过坐标被maven管理。
- maven仓库包括本地仓库和远程仓库。优先从本地仓库（本地仓库默认地址为：${user.home}/.m2/repository。）中寻找相关构件，远程仓库中的构件下载到本地仓库使用。中央仓库是 Maven 核心自带的远程仓库，默认地址：http://repo1.maven.org/maven2。私服是架设在本机或局域网中的一种特殊的远程仓库，通过私服可以方便的管理其它所有的外部远程仓库。
- 本地仓库的配置
![这里写图片描述](http://img.blog.csdn.net/20180224154537235?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180224154546823?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 中央仓库配置（maven默认配置好的）
![这里写图片描述](http://img.blog.csdn.net/20180224154952435?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](http://img.blog.csdn.net/20180224155002822?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 镜像仓库的配置
![这里写图片描述](http://img.blog.csdn.net/20180224155626947?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 私服的搭建
大多数公司使用私服，暂时先写到这，后面抽时间再介绍搭建私服