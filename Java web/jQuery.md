# jQuery
最近的项目用到jQuery,但之前我对前端的东西不是很熟，故参照W3CSchool的教程进行总结和笔记记录，加深印象。

-------------------
##1. jQuery是一个轻量级的javaScript函数库
- jQuery包含以下特性

	- HTML元素的选取
	- HTML元素的操作
	- CSS操作
	- HTML事件函数
	- JavaScript特效和动画
	- HTML DOM遍历和修改
	- AJAX
	- Utilities
		 
- jQuery库位于单个JavaScript文件中，其中包含所有jQuery函数。引入jQuery库：
			/*<head>
				<script type="text/javascript" src="jquery.js"></script>
			</head>*/
实际开发中可以这样引入
	
		<%
		    String path = request.getContextPath();
		 %>
		 <script type="text/javascript" src="<%=path%>/edview/js/jquery/jquery.js"></script>
		 //<%=path%>后面是jQuery.js实际的存放位置
如果不愿意在自己的计算机上存放 jQuery 库，那么可以从 Google or Microsoft 加载 CDN jQuery 核心文件。请参考问W3CSchool教程。

-------------------
## 2.文档就绪函数
	
	$(document).ready(function(){

		--- jQuery functions go here ----

	});

-------------------
实例中所有的jQuery函数都应该在文档就绪函数中，这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码。

-------------------
## 3.jQuery选择器
- 元素选择器（使用css选择器来选取HTML元素）
	- $("p") 选取 <p> 元素。

	- $("p.intro") 选取所有 class="intro" 的 <p> 元素。

	- $("p#demo") 选取 id="demo" 的第一个 <p> 元素
- jQuery 属性选择器（使用 XPath 表达式来选择带有给定属性的元素）
	- $("[href]") 选取所有带有 href 属性的元素。

	- $("[href='#']") 选取所有带有 href 值等于 "#" 的元素。

	- $("[href!='#']") 选取所有带有 href 值不等于 "#" 的元素。

	- $("[href$='.jpg']") 选取所有 href 值以 ".jpg" 结尾的元素。
- jQuery CSS 选择器
	
	- $("p").css("background-color","red");
	
-------------------
##  4.jQuery事件
- 事件处理函数是当 HTML 中发生事件时自动被调用的函数。由“事件”（event）“触发”（triggered）是经常被用到的术语。
- jQuery的书写原则
		
	- 把所有 jQuery 代码置于事件处理函数中 
	- 把所有事件处理函数置于文档就绪事件处理器中 
	- 把 jQuery 代码置于单独的 .js 文件中 
	- 如果存在名称冲突，则重命名 jQuery 库 

-------------------
##  5. jQuery效果函数
	隐藏、显示、切换、以及动画效果。常见的：
	
- show();
- hide();
- toggle();

-------------------
## 6. jQuery Callback回调函数
- A callback is a function that is passed as an argument to another function and is executed after its parent function has completed.回调函数就是一个函数被当做参数传入另一个函数中，当另一个函数执行完成后，该函数被执行。例：
	
		$("p").hide(1000,function(){
			alert("The paragraph is now hidden");
		});
- 回调函数的理解
		
		var fn = new Function("arg1", "arg2", "return arg1 * arg2;");
		fn(3, 4);   //12
		上面这段代码中函数的参数是字符串，最后一段字符串实际上是一段js代码，因此上一段代码中，参数function()实际上相当于一段可执行的字符串，最后会被执行。
- 应用。在涉及到动画等函数处理时，回调函数很有用。例：
		
		/*<p>The paragraph</p>*/
		
			$(document).ready(function(){
				  $("button").click(function(){
					  $("p").hide(4000);
					 alert("The paragraph is now hidden");
				  });
			});
点击button后，弹出标签被隐藏的信息，但实际上p标签在4秒后隐藏，可能还未隐藏，这是就发生了逻辑上的错误，因此需要把alert("The paragraph is now hidden")写到callback函数中，如下
	
			$(document).ready(function(){
				  $("button").click(function(){
					  $("p").hide(1000,function(){
						    alert("The paragraph is now hidden");
					    });
				  });
			});

-------------------
## 7.jQuery操作HTML
- 改变HTML内容
		
		$(selector).html(content);  //html()函数改变所匹配的HTML元素的内容（innerHTML）
-  添加HTML内容
		
		$(selector).append(content); //append()函数向所匹配的HTML元素内部追加内容

-------------------
## 8.jQuery CSS函数
		
- $(selector).css(name,value)   //为所有匹配元素的给定CSS属性设置值
- $(selector).css({properties})   //为所有匹配元素的一系列CSS属性设置值
- $(selector).css(name)  //返回指定CSS属性的值
		
		$("p").css("background-color","yellow");
		$("p").css({"background-color":"yellow","font-size":"200%"});
		$(this).css("background-color");

- jQuery 拥有两种供尺寸操作的重要函数：

		$(selector).height(value) 
		$(selector).width(value) 
		
-------------------
##  9.jQuery AJAX函数
	
- jQuery提供了供AJAX开发的丰富函数库
- jQuery 的 load 函数是一种简单的（但很强大的）AJAX 函数
		
		$(selector).load(url,data,callback)
- $.ajax(options) 是低层级 AJAX 函数的语法，提供了比高层级函数更多的功能，但是同时也更难使用。
		option 参数设置的是 name|value 对，定义 url 数据、密码、数据类型、过滤器、字符集、超时以及错误函数
	
				var params = {//三个参数为
		    			'task_Name': task_Name,
		    			'business_Type':business_Type,
		    			'send_Schedule':send_Schedule,
		                };
		    	
		    	$.ajax({
		    		url:  path + '/task/queryAllTaskList',
		    		type : 'post', /*请求方式*/
		    		dataType : 'json', /*被返回的数据的类型 (html,xml,json,jasonp,script,text)*/
		    		data : params,/*键值对的参数*/
		    		success : function(data) {//data是请求后
		    			
		    			showTerminalTalble(data.taskList);
		    			if(null == data.errMsg || data.errMsg == "") {
		                    if(updatePageNum){
		                        paginationUtil.showPageCountSelect(data.totalNum);
		                    }
		                    if(clickbtn){
		                        paginationUtil.setPageNum(1);
		                    }	
		    			 } else {
		    				$("#taskTable").html("");
		    				$.jBoxUtil.noticeError({content: data.errMsg});
		    			}
		    			 				
		    		},
		    		error : function(data) {
		    			//var b=1;
		    		}
		    	});	 
		    }
	
-------------------



