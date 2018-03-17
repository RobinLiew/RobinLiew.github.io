最近开发一个项目用到AngularJS，作为一个java后端程序员，有点赶鸭子上架的赶脚，万幸AngularJS这个前端框架上手比较快，我参照 http://www.runoob.com/angularjs/angularjs-tutorial.html 网站上的教程进行学习，并对其进行笔记总结，加深记忆。
### 1.AngularJS基本介绍
#### AngularJS是什么？

AngularJS是谷歌的一块前端JS框架。它是一个以 JavaScript 编写的库，是为了克服HTML在构建应用上的不足而设计的。它有着诸多的核心特性：MVC、模块化、自动化双向数据绑定、语义化标签、依赖注入等等。如果你和我一样是Java后端程序员的话，相信对MVC，依赖注入等特性会很耳熟，学习起来也很容易。AngularJS弥补了HTML在构建应用方面的不足，使开发者可以使用HTML来声明动态内容，从而使得Web开发和测试工作变得更加容易。

#### AngularJS能做什么？

- AngularJS 使得开发现代的单一页面应用程序（SPAs：Single Page Applications）变得更加容易。
 AngularJS擅长构建一个CRUD（增加Create、查询Retrieve、更新Update、删除Delete）的应用
    - AngularJS 把应用程序数据绑定到 HTML 元素。
    - AngularJS 可以克隆和重复 HTML 元素。
    - AngularJS 可以隐藏和显示 HTML 元素。
    - AngularJS 可以在 HTML 元素"背后"添加代码。
    - AngularJS 支持输入验证。
### 2.Angular基础（一） 

#### AngularJS指令
	
- AngularJS 通过 ng-directives 扩展了 HTML。
	- 创建模块：你可以通过 AngularJS 的 angular.module 函数来创建模块
	- 添加控制器：你可以使用 ng-controller 指令来添加应用的控制器
	- 添加指令：使用directive来为你应用添加自己的指令
- ng-app 指令定义一个 AngularJS 应用程序。
- ng-model 指令把元素值（比如输入域的值）绑定到应用程序。
	ng-model 指令可以将输入域的值与 AngularJS 创建的变量绑定。
	- 1.双向绑定：在修改输入域的值时， AngularJS 属性的值也将修改
		

		```
		<div ng-app="myApp" ng-controller="myCtrl">
		    名字: <input ng-model="name">
		    <h1>你输入了: {{name}}</h1>
		</div>
		```
	- 2.验证用户输入
		

		```
		<form ng-app="" name="myForm">
		    Email:
		    <input type="email" name="myAddress" ng-model="text">
		    <span ng-show="myForm.myAddress.$error.email">不是一个合法的邮箱地址</span>
		    <!-- 提示信息会在 ng-show 属性返回 true 的情况下显示 -->
		</form>
		```
		```
		一个完整的验证实例：
		 <!DOCTYPE html>
		<html>
		<script src="http://apps.bdimg.com/libs/angular.js/1.4.6/angular.min.js"></script>
		<body>
		
		<h2>Validation Example</h2>
		
		<form  ng-app="myApp"  ng-controller="validateCtrl"
		name="myForm" novalidate>
		
		<p>用户名:<br>
		  <input type="text" name="user" ng-model="user" required>
		  <span style="color:red" ng-show="myForm.user.$dirty && myForm.user.$invalid">
		  <span ng-show="myForm.user.$error.required">用户名是必须的。</span>
		  </span>
		</p>
		
		<p>邮箱:<br>
		  <input type="email" name="email" ng-model="email" required>
		  <span style="color:red" ng-show="myForm.email.$dirty && myForm.email.$invalid">
		  <span ng-show="myForm.email.$error.required">邮箱是必须的。</span>
		  <span ng-show="myForm.email.$error.email">非法的邮箱。</span>
		  </span>
		</p>
		
		<p>
		  <input type="submit"
		  ng-disabled="myForm.user.$dirty && myForm.user.$invalid ||
		  myForm.email.$dirty && myForm.email.$invalid">
		</p>
		
		</form>
		
		<script>
		var app = angular.module('myApp', []);
		app.controller('validateCtrl', function($scope) {
		    $scope.user = 'John Doe';
		    $scope.email = 'john.doe@gmail.com';
		});
		</script>
		
		</body>
		</html> 
		```
	- 3.应用状态

		ng-model 指令可以为应用数据提供状态值(invalid, dirty, touched, error):
			

		```
		<form ng-app="" name="myForm" ng-init="myText = 'test@runoob.com'">
		    Email:
		    <input type="email" name="myAddress" ng-model="myText" required></p>
		    <h1>状态</h1>
		    {{myForm.myAddress.$valid}}
		    {{myForm.myAddress.$dirty}}
		    {{myForm.myAddress.$touched}}
		</form>
		<!--
			下面这些验证的值为布尔类型，可以结合控制器进行非常灵活的操作
			ng-valid: 验证通过
			ng-invalid: 验证失败
			ng-valid-[key]: 由$setValidity添加的所有验证通过的值
			ng-invalid-[key]: 由$setValidity添加的所有验证失败的值
			ng-pristine: 控件为初始状态
			ng-dirty: 控件输入值已变更
			ng-touched: 控件已失去焦点
			ng-untouched: 控件未失去焦点
			ng-pending: 任何为满足$asyncValidators的情况
		-->
		```
	- 4.CSS 类
		ng-model 指令基于它们的状态为 HTML 元素提供了 CSS 类


		```
		 <style>
			input.ng-invalid {
			    background-color: lightblue;
			}
		</style>
		<body>
		
			<form ng-app="" name="myForm">
			    输入你的名字:
			    <input name="myAddress" ng-model="text" required>
			</form>
		</body>
		```
		

- ng-bind 指令把应用程序数据绑定到 HTML 视图。
	
	
	- 实现数据绑定
		通过数据绑定同步了AngularJS数据与表达式
	```
	
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="utf-8">
	<!-- 这里AngularJS库可以下载下来引入，下载地址为https://github.com/angular/angular.js/releases -->
	<script src=".../angular.min.js"></script>
	</head>
	<body>
	 
	<div ng-app="">
	     <p>名字 : <input type="text" ng-model="name"></p>
	     <h1>Hello {{name}}</h1> <!-- 双括号为AngularJS 表达式 -->
	     <!-- 注意，这里把上面的元素h1换成<h1 ng-bind="firstName"></h1> 效果是一样的 -->
	</div>
	 
	</body>
	</html>

	```
		运行结果如下图所示，数据传入ng-model的变量name中，通过表达式在h1元素中显示出来。
	![这里写图片描述](http://img.blog.csdn.net/20180308084500101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
	说明：
	```
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="utf-8">
	<!-- 这里AngularJS库可以下载下来引入，下载地址为https://github.com/angular/angular.js/releases -->
	<script src=".../angular.min.js"></script>
	</head>
	<body>
	下面如果不做特殊说明的话，所有的HTML元素代码片段都是要放入这里运行，为了简化代码，以下的片段就不重复写这些了。
	</body>
	</html>
	```

- ng-repeat指令：会重复一个 HTML 元素
	
	```
		
	<div ng-app="" ng-init="names=['Jani','Hege','Kai']">
		<p>使用 ng-repeat 来循环数组</p>
		<ul>
		    <li ng-repeat="x in names">
		      {{ x }}
		    </li>
		</ul>
	</div>
	
	```
		运行结果如下所示：
	![这里写图片描述](http://img.blog.csdn.net/20180308093426552?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
	ng-repeat 指令用在一个对象数组上：
	
	```
		
	<div ng-app="" ng-init="names=[
	{name:'Jani',country:'Norway'},
	{name:'Hege',country:'Sweden'},
	{name:'Kai',country:'Denmark'}]">
		 
	<p>循环对象：</p>
	<ul>
		 <li ng-repeat="x    in names">
		   {{ x.name + ', ' + x.country }}
		 </li>
	</ul>
		 
	</div>
	
	```
		
	运行结果：
		
![这里写图片描述](http://img.blog.csdn.net/20180308093721519?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


- 创建自定义的指令
	除了 AngularJS 内置的指令外，我们还可以创建自定义指令。
	你可以使用 .directive 函数来添加自定义的指令。
	要调用自定义指令，HTML 元素上需要添加自定义指令名。
	使用驼峰法来命名一个指令， runoobDirective, 但在使用它时需要以 - 分割, runoob-directive:
	且你可以通过元素名、属性、类名、注释来调用指令。

	```
	<body ng-app="myApp">

	<runoob-directive></runoob-directive><!-- 元素名 -->
	<!-- <div runoob-directive></div> --> <!-- 属性 -->
	<!-- <div class="runoob-directive"></div> --> <!-- 类名 -->
	<!-- <!-- directive: runoob-directive --> --> <!-- 注释 -->
	<script>
		var app = angular.module("myApp", []);
		app.directive("runoobDirective", function() {
		    return {
			  /*restrict : "A",*/  
		        template : "<h1>自定义指令!</h1>"
		    };
		});
		/*
		过添加 restrict 属性,并设置值为 "A", 来设置指令只能通过属性的方式来调用
		restrict 值可以是以下几种:

		    E 作为元素名使用
		    A 作为属性使用
		    C 作为类名使用
		    M 作为注释使用
		
		restrict 默认值为 EA, 即可以通过元素名和属性名来调用指令
		*/
	</script>
	
	</body>
	```

#### AngularJS 表达式

AngularJS 表达式写在双大括号内：{{ expression }}。
AngularJS 表达式把数据绑定到 HTML，这与 ng-bind 指令有异曲同工之妙。
AngularJS 将在表达式书写的位置"输出"数据。
AngularJS 表达式 很像 JavaScript 表达式：它们可以包含文字、运算符和变量。
实例 {{ 5 + 5 }} 或 {{ firstName + " " + lastName }}
	
- 1.AngularJS 数字：AngularJS 数字就像 JavaScript 数字

	```
	<!--  注意：ng-init在初始化数据时并不常用，后面会使用控制器模块代替它的方法-->
	<div ng-app="" ng-init="quantity=1;cost=5">
	 <!-- 双大括号内会直接计算出两个变量的乘积，并显示在页面上 -->
	<p>总价： {{ quantity * cost }}</p>
	 
	</div>

	```

- 2.AngularJS 字符串：AngularJS 字符串就像 JavaScript 字符串
	

	```
	
	<div ng-app="" ng-init="firstName='John';lastName='Doe'">
	 
	<p>姓名： {{ firstName + " " + lastName }}</p>
	 
	</div>

	```
- 3.AngularJS 对象：AngularJS 对象就像 JavaScript 对象
	

	```
	<div ng-app="" ng-init="person={firstName:'John',lastName:'Doe'}">
	 
	<p>姓为 {{ person.lastName }}</p>
	 
	</div>

	```
- 4.AngularJS 数组：AngularJS 数组就像 JavaScript 数组
	

	```
	
	<div ng-app="" ng-init="points=[1,15,19,2,40]">
	 
	<p>第三个值为 {{ points[2] }}</p>
	 
	</div>

	```
	 注意AngularJS数字、字符串、对象写法的区别。
		- 数字和字符串都是包裹在" "中，
		- 对象的写法为  对象名={...}，
		- 数组的写法为  数组名=[...]

#### AngularJS 应用
	
AngularJS 模块（Module） 定义了 AngularJS 应用。
AngularJS 控制器（Controller） 用于控制 AngularJS 应用。
ng-app指令指明了应用, ng-controller 指明了控制器。
	
	

```
	<!-- 要运行的话把这段代码放入完整的HTML页面的body元素中 -->
	<div ng-app="myApp" ng-controller="myCtrl">
	 
	名: <input type="text" ng-model="firstName"><br>
	姓: <input type="text" ng-model="lastName"><br>
	<br>
	姓名: {{firstName + " " + lastName}}
	 
	</div>
	 
	<script>
	var app = angular.module('myApp', []);//定义AngularJS模块
	//使用AngularJS控制器控制应用
	app.controller('myCtrl', function($scope) {
	    $scope.firstName= "John";
	    $scope.lastName= "Doe";
	});
	</script>

```

- AngularJS加载多个ng-app
	上面的例子中，我们发现ng-app只出现了一次，如果出现多次会怎么样呢？
	 ng-app是一个特殊的指令，一个HTML文档只出现一次，如出现多次也只有第一个起作用；除非我们手动加载除第一个ng-app外的其他ng-app。
	 ng-app可以出现在html文档的任何一个元素上。
 

```
	<div ng-app="myApp" ng-controller="myCtrl">  
	  
	<h1>姓氏为 {{lastname}} 家族成员:</h1>  
	  
	<ul>  
	    <li ng-repeat="x in names">{{x}} {{lastname}}</li>  
	</ul>  
	  
	</div>  
	      
	<div id="myApp2" ng-app="myApp2" ng-controller="myCtrl">  
	<h1>myApp2 姓氏为 {{lastname}} 家族成员:</h1>  
	<ul>  
	    <li ng-repeat="x in names">{{x}}</li>  
	</ul>  
	</div>  
	  
	<div id="myApp3" ng-app="myApp3" ng-controller="myCtrl">  
	<h1>myApp3 姓氏为 {{lastname}} 家族成员:</h1>  
	<input type="button" value="rootScope共享" ng-click="say()">  
	</div>

	<script>
		var app = angular.module('myApp', []);  
		app.controller('myCtrl', function($scope, $rootScope) {  
		    $scope.names = ["Emil", "Tobias", "Linus"];  
		    //$rootScope.lastname = "Refsnes";  
		});  
		      
		var app2 = angular.module('myApp2', []);  
		app2.controller('myCtrl', function($scope, $rootScope) {  
		        $scope.names = ["Emil2", "Tobias2", "Linus2"];  
		    $rootScope.lastname = "Refsnes--2";  
		});  
		  
		var app3 = angular.module('myApp3', []);  
		app3.controller('myCtrl', function($scope, $rootScope) {  
		    $rootScope.lastname = "Refsnes--3";  
		    $rootScope.say = function(){  
		            $rootScope.lastname = "Refsnes---";  
		        }  
		});  
		  
		//手动加载myApp2 ng-app  
		angular.bootstrap(document.getElementById("myApp2"), ['myApp2']);  
		angular.bootstrap(document.getElementById("myApp3"), ['myApp3']);
	</script>
```

#### Scope(作用域)
AngularJS 应用组成如下：

    View(视图), 即 HTML。
    Model(模型), 当前视图中可用的数据。
    Controller(控制器), 即 JavaScript 函数，可以添加或修改属性。

- scope 是模型。
scope 是一个 JavaScript 对象，带有属性和方法，这些属性和方法可以在视图和控制器中使用
	
	
	```
	<div ng-app="myApp" ng-controller="myCtrl">
			
		<h1>{{carname}}</h1>
			
	</div>
			
	<script>
		var app = angular.module('myApp', []);
				
		app.controller('myCtrl', function($scope) {
			$scope.carname = "Volvo";
		});
		/*
			当在控制器中添加 $scope 对象时，视图 (HTML) 可以获取了这些属性。
			视图中，你不需要添加 $scope 前缀, 只需要添加属性名即可，如： {{carname}}
		*/
	</script>
			
	```
- 根作用域
	所有的应用都有一个 \$rootScope，它可以作用在 ng-app 指令包含的所有 HTML 元素中。
$rootScope 可作用于整个应用中。是各个 controller 中 scope 的桥梁。用 rootscope 定义的值，可以在各个 controller 中使用。

	```
	
	<div ng-app="myApp" ng-controller="myCtrl">
	
	<h1>{{lastname}} 家族成员:</h1>
	
	<ul>
	    <li ng-repeat="x in names">{{x}} {{lastname}}</li>
	</ul>
	
	</div>
	
	<script>
	var app = angular.module('myApp', []);
	
	app.controller('myCtrl', function($scope, $rootScope) {
	    $scope.names = ["Emil", "Tobias", "Linus"];
	    $rootScope.lastname = "Refsnes";
	});
	</script>
	```
#### AngularJS 控制器
- AngularJS 控制器 控制 AngularJS 应用程序的数据.
 AngularJS 控制器是常规的 JavaScript 对象。AngularJS 使用\$scope 对象来调用控制器。在 AngularJS 中， \$scope 是一个应用对象(属于应用变量和函数)。控制器的 $scope （相当于作用域、控制范围）用来保存AngularJS Model(模型)的对象

-  在大型的应用程序中，通常是把控制器存储在外部文件中。只需要把 script 标签中的代码复制到名为xxx.js 的外部文件中即可
	
	```
	<div ng-app="myApp" ng-controller="personCtrl">

	名: <input type="text" ng-model="firstName"><br>
	姓: <input type="text" ng-model="lastName"><br>
	<br>
	姓名: {{fullName()}}
	
	</div>
	
	<script>
	var app = angular.module('myApp', []);
	app.controller('personCtrl', function($scope) {
	    $scope.firstName = "John";   //控制器属性
	    $scope.lastName = "Doe";    //控制器属性
	    $scope.fullName = function() {  //控制器方法
	        return $scope.firstName + " " + $scope.lastName;
	    }
	});
	</script> 
	```
#### AngularJS 过滤器
- 过滤器可以使用一个管道字符（|）添加到表达式和指令中
常用的过滤器
	- currency 	格式化数字为货币格式。
	- filter 	从数组项中选择一个子集。
	- lowercase 	格式化字符串为小写。
	- orderBy 	根据某个表达式排列数组。
	- uppercase 	格式化字符串为大写。
	
	

	```
	1、uppercase，lowercase 大小写转换

	{{ "lower cap string" | uppercase }}   // 结果：LOWER CAP STRING
	{{ "TANK is GOOD" | lowercase }}      // 结果：tank is good
	
	2、date 格式化
	
	{{1490161945000 | date:"yyyy-MM-dd HH:mm:ss"}} // 2017-03-22 13:52:25
	
	3、number 格式化（保留小数）
	
	{{149016.1945000 | number:2}}
	
	4、currency货币格式化
	
	{{ 250 | currency }}            // 结果：$250.00
	{{ 250 | currency:"RMB ￥ " }}  // 结果：RMB ￥ 250.00
	
	5、filter查找
	
	输入过滤器可以通过一个管道字符（|）和一个过滤器添加到指令中，该过滤器后跟一个冒号和一个模型名称。
	
	filter 过滤器从数组中选择一个子集
	
	 // 查找name为iphone的行
	<!-- 
	       
	-->
	
	6、limitTo 截取
	
	{{"1234567890" | limitTo :6}} // 从前面开始截取6位
	{{"1234567890" | limitTo:-4}} // 从后面开始截取4位
	
	7、orderBy 排序
	
	 // 根id降序排
	
	
	// 根据id升序排
	
	```

	```
	<!-- 表达式中添加过滤器 -->
	 <div ng-app="myApp" ng-controller="personCtrl">

		<p>姓名为 {{ lastName | uppercase }}</p><!--  uppercase 过滤器将字符串格式化为大写 -->

	</div> 
	```
	

	```
	 <div ng-app="myApp" ng-controller="costCtrl">

	<input type="number" ng-model="quantity">
	<input type="number" ng-model="price">
	
	<p>总价 = {{ (quantity * price) | currency }}</p><!-- currency 过滤器将数字格式化为货币格式 -->
	
	</div> 
	```

	```
	 <div ng-app="myApp" ng-controller="namesCtrl">

	<ul>
	  <li ng-repeat="x in names | orderBy:'country'"> <!-- 向指令添加过滤器。过滤器可以通过一个管道字符（|）和一个过滤器添加到指令中。orderBy 过滤器根据表达式排列数组 -->
	    {{ x.name + ', ' + x.country }}
	  </li>
	</ul>
	
	</div> 
	```
	
	

	```
	<!-- 自定义过滤器:以下实例自定义一个过滤器 reverse，将字符串反转 -->
	var app = angular.module('myApp', []);
	app.controller('myCtrl', function($scope) {
	    $scope.msg = "Runoob";
	});
	app.filter('reverse', function() { //可以注入依赖
	    return function(text) {
	        return text.split("").reverse().join("");
	    }
	});

	```
	
### AngularJS基础（二）
#### AngularJS 服务(Service)
- 在 AngularJS 中，服务是一个函数或对象，可在你的 AngularJS 应用中使用。
AngularJS 内建了30 多个服务，且你可以自己建立服务。
- $location 服务，它可以返回当前页面的 URL 地址

	```
	<!--  与window.location 对象相似，但在AngularJS中$location更好用 -->
	var app = angular.module('myApp', []);
	app.controller('customersCtrl', function($scope, $location) {
	    $scope.myUrl = $location.absUrl();
	}); 
	```
- \$http 服务
\$http 是 AngularJS 应用中最常用的服务。 服务向服务器发送请求，应用响应服务器传送过来的数据。
$http 是 AngularJS 中的一个核心服务，用于读取远程服务器的数据。

- 使用格式：

	```
		//标准格式
		// 简单的 GET 请求，可以改为 POST
		$http({
		    method: 'GET',
		    url: '/someUrl'
		}).then(function successCallback(response) {
		        // 请求成功执行代码
		    }, function errorCallback(response) {
		        // 请求失败执行代码
		});
		
		//例：
		var app = angular.module('myApp', []);
		app.controller('myCtrl', function($scope, $http) {
		    $http.get("welcome.htm").then(function (response) {
		        $scope.myWelcome = response.data;
		    });
		});
	```
- \$timeout 服务
AngularJS $timeout 服务对应了 JS window.setTimeout 函数。

	```
	var app = angular.module('myApp', []);
	app.controller('myCtrl', function($scope, $timeout) {
	    $scope.myHeader = "Hello World!";
	    $timeout(function () {
	        $scope.myHeader = "How are you today?";
	    }, 2000);//两秒后显示"How are you today?"信息
	});
	```
- \$interval 服务
AngularJS $interval 服务对应了 JS window.setInterval 函数。

	```
	var app = angular.module('myApp', []);
	app.controller('myCtrl', function($scope, $interval) {
	    $scope.theTime = new Date().toLocaleTimeString();
	    $interval(function () {
	        $scope.theTime = new Date().toLocaleTimeString();
	    }, 1000);//每隔一秒显示一次时间信息
	});
	```
- 创建自定义服务

	```
	//创建名为hexafy 的服务
	app.service('hexafy', function() {
	    this.myFunc = function (x) {
	        return x.toString(16);
	    }
	});

	使用自定义的的服务 hexafy 将一个数字转换为16进制数:
	app.controller('myCtrl', function($scope, hexafy) {//注意自定义服务传入时，没有$符号
	    $scope.hex = hexafy.myFunc(255);
	});

	```
- 过滤器中，使用自定义服务

	```
	app.filter('myFormat',['hexafy', function(hexafy) {
	    return function(x) {
	        return hexafy.myFunc(x);
	    };
	}]);

	//使用我定义的过滤器
	<ul>
	<li ng-repeat="x in counts">{{x | myFormat}}</li>
	</ul>
	```
	
#### AngularJS Select(选择框)
- 在 AngularJS 中我们可以使用 ng-option 指令来创建一个下拉列表，列表项通过对象和数组循环输出
	ng-repeat 指令是通过数组来循环 HTML 代码来创建下拉列表，但 ng-options 指令更适合创建下拉列表，它有以下优势：
使用 ng-options 的选项是一个对象， ng-repeat 是一个字符串。当选择值是一个对象时，我们就可以获取更多信息，应用也更灵活。
	```
	
	<div ng-app="myApp" ng-controller="myCtrl">
	 
	<select ng-init="selectedName = names[0]" ng-model="selectedName" ng-options="x for x in names">
	</select>
	 
	</div>
	 
	<script>
	var app = angular.module('myApp', []);
	app.controller('myCtrl', function($scope) {
	    $scope.names = ["Google", "Runoob", "Taobao"];
	});
	</script>


	//将数据对象作为数据源
	$scope.sites = {
	    site01 : "Google",
	    site02 : "Runoob",
	    site03 : "Taobao"
	};

	//使用对象作为数据源, x 为键(key), y 为值(value):
	<select ng-model="selectedSite" ng-options="x for (x, y) in sites">
	</select>
	
	<h1>你选择的值是: {{selectedSite}}</h1>
	```
#### AngularJS HTML DOM
- ng-disabled 指令
ng-disabled 指令直接绑定应用程序数据到 HTML 的 disabled 属性。

	```
	<div ng-app="" ng-init="mySwitch=true">

	<p>
	<button ng-disabled="mySwitch">点我!</button>
	</p>
	
	<p>
	<input type="checkbox" ng-model="mySwitch">按钮 <!-- mySwitch的值为true时，button将不可操作 -->
	</p>
	
	<p>
	{{ mySwitch }}
	</p>
	
	</div> 
	```
- ng-show 指令
ng-show 指令隐藏或显示一个 HTML 元素。

	```
	<div ng-app="">

	<p ng-show="true">我是可见的。</p>
	
	<p ng-show="false">我是不可见的。</p>
	
	</div> 
	```
- ng-hide 指令
ng-hide 指令用于隐藏或显示 HTML 元素。

	```
	 <div ng-app="">

		<p ng-hide="true">我是不可见的。</p>
		
		<p ng-hide="false">我是可见的。</p>
	
	</div> 
	```
#### AngularJS 事件
- ng-click 指令定义了 AngularJS 点击事件

	```
	<div ng-app="" ng-controller="myCtrl">
	
	<button ng-click="count = count + 1">点我！</button>
	
	<p>{{ count }}</p>
	
	</div>
	```

	```
	<!-- 控制显示 HTML 元素 -->
	<div ng-app="myApp" ng-controller="personCtrl">

	<button ng-click="toggle()">隐藏/显示</button>
	
	<p ng-show="myVar">
	名: <input type="text" ng-model="firstName"><br>
	姓: <input type="text" ng-model="lastName"><br>
	<br>
	姓名: {{firstName + " " + lastName}}
	</p>
	
	</div>
	
	<script>
	var app = angular.module('myApp', []);
	app.controller('personCtrl', function($scope) {
	    $scope.firstName = "John",
	    $scope.lastName = "Doe"
	    $scope.myVar = true;
	    $scope.toggle = function() {
	        $scope.myVar = !$scope.myVar;
	    }
	});
	</script> 
	```
#### AngularJS API
 - AngularJS 全局 API 用于执行常见任务的 JavaScript 函数集合，如：

    - 比较对象
    - 迭代对象
    - 转换对象
全局 API 函数使用 angular 对象进行访问。
	- angular.lowercase() 	转换字符串为小写
	- angular.uppercase() 	转换字符串为大写
	- angular.isString() 	判断给定的对象是否为字符串，如果是返回 true。
	- angular.isNumber() 	判断给定的对象是否为数字，如果是返回 true。
	
#### AngularJS 包含
- 使用 AngularJS, 你可以使用 ng-include 指令来包含 HTML 内容:

	```
	
	<body ng-app="">
	 
	<div ng-include="'runoob.htm'"></div>
	 
	</body>

	```
- 包含 AngularJS 代码

	```
	sites.htm文件中的内容：
	
	<table>
	<tr ng-repeat="x in names">
	<td>{{ x.Name }}</td>
	<td>{{ x.Url }}</td>
	</tr>
	</table>

	
	<div ng-app="myApp" ng-controller="sitesCtrl"> 
	  <div ng-include="'sites.htm'"></div><!-- 引入sites.htm文件 -->
	</div>
	 
	<script>
	var app = angular.module('myApp', []);
	app.controller('sitesCtrl', function($scope, $http) {
	    $http.get("sites.php").then(function (response) {
	        $scope.names = response.data.records;
	    });
	});
	</script>

	```
#### AngularJS 动画
- AngularJS 提供了动画效果，可以配合 CSS 使用。ngAnimate 模型可以添加或移除 class 。ngAnimate 模型并不能使 HTML 元素产生动画，但是 ngAnimate 会监测事件，类似隐藏显示 HTML 元素 ，如果事件发生 ngAnimate 就会使用预定义的 class 来设置 HTML 元素的动画。
AngularJS 使用动画需要引入 angular-animate.min.js 库。

	```
	<script src="../angular-animate.min.js"></script>
	```
	```
	<body ng-app="ngAnimate">

	隐藏 DIV: <input type="checkbox" ng-model="myCheck">
	
	<div ng-hide="myCheck"></div>
	
	</body>
	```
如果我们应用已经设置了应用名，可以把 ngAnimate 直接添加在模型中：

	```
	
	<body ng-app="myApp">
	
	<h1>隐藏 DIV: <input type="checkbox" ng-model="myCheck"></h1>
	
	<div ng-hide="myCheck"></div>
	
	<script>
	var app = angular.module('myApp', ['ngAnimate']);
	</script>
	```
#### AngularJS 依赖注入
- AngularJS 提供很好的依赖注入机制。以下5个核心组件用来作为依赖注入：

   -  value
   -  factory
   -  service
   -  provider
   - constant

- value

	Value 是一个简单的 javascript 对象，用于向控制器传递值（配置阶段）： 

	```
	// 定义一个模块
	var mainApp = angular.module("mainApp", []);
	
	// 创建 value 对象 "defaultInput" 并传递数据
	mainApp.value("defaultInput", 5);
	...
	
	// 将 "defaultInput" 注入到控制器
	mainApp.controller('CalcController', function($scope, CalcService, defaultInput) {
	   $scope.number = defaultInput;
	   $scope.result = CalcService.square($scope.number);
	   
	   $scope.square = function() {
	      $scope.result = CalcService.square($scope.number);
	   }
	});
	```
剩下的理解更深入一步再写。
	

