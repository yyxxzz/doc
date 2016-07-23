# 前言 #
## 编写目的：##
        基于这次CRM项目选用bootstrap作为前台框架。但是使用原生态的bootstrap需要在页面上写N多的div，table等标签。
        这个就需要对bootstrap的样式，风格有很深的了解的人才能去使用。基于这样的一个背景，我这里对bootstrap的一些常用框架做了jquery风格的插件封装
        现在市面上关于bootstrap的插件很多，但是没有一家对bootstrap插件做一个完整的封装。



## 插件优点： ##

 - 完全采用jquery风格插件的方式统一封装。
 - 使用简单，完全面向对象的设计。
 - 插件文档齐全。

**该文档基于插件的三大要素（属性，事件，方法）来详细描述每个插件的使用方法**

## 插件列表 ##
 - [1、导航菜单（accordion）](#1显示一个导航菜单accordion)
 - [2、告警框（alerts）](#2显示一个警告框alerts)
 - [3、面包屑导航（breadcrumb）](#3显示一个面包屑导航breadcrumb)
 - [4、组合框（combo）](#4显示一个下组合框combo)
 - [5、组合下拉框（combobox）](#5显示一个下组合下拉框combobox)
 - [6、组合下拉表格（combotable）](#6显示一个下组合下拉表格combotable)
 - [7、模态对话框（dialog）](#7显示一个模态对话框dialog)
 - [8、输入表单（form）](#8显示一个输入表单form)
 - [9、菜单（menu）](#9显示一个菜单menu)
 - [10、分页（pagination）](#10显示一个分页插件pagination)
 - [11、面板（panel）](#11显示一个面板插件panel)


## 枚举定义 ##

1、面板样式
 - panel-primary
 - panel-success
 - panel-info
 - panel-warning
 - panel-danger
    
2、警告框类型
 - default
 - primary
 - success
 - info
 - warning
 - danger
 
3、按钮样式
 - btn-default
 - btn-primary
 - btn-success
 - btn-info
 - btn-warning
 - btn-danger

    

----------

# 插件介绍 #
## 1、显示一个导航菜单（accordion）##

显示一个导航菜单

依赖于：无
继承于：无

使用举例
```html
<ul id="leftMenu"></ul>
```

```javascript
    $("#leftMenu").accordion({
	    panelClass : "panel-primary",
		idField : "menuId",
		pidField : "parentMenuId",
		textField : "menuName",
		ajaxParam : {
			dataType : "json",
			url: "/SysMenuController/getSysMenuList.do",
			type: "POST"
		},
		onClickMenu : function(item, parent) {
			$("#body").load("/crm" + item.menuUrl);
		}
	});
```

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.accordion.js

**该插件可供选择的属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|panelClass   |string    |是  | "panel-default" |面板样式（可选值见枚举定义1）|
|rootPid   |int    |否  |0 |最外层节点的pid值|
|idField   |string    |是  |"id" |对象中id的属性名称|
|pidField   |string    |是  |"pid" |对象中pid的属性名称|
|textField   |string    |是  |"text" |对象中显示在菜单中的名称的属性|
|ajaxParam   |object    |是  |{} |从后端获取数据的ajax的对象|
|dataList   |array    |是  |[] |需要显示的的对象数组（ajaxParam和dataList不需要同时使用，如果同时有值，则以ajaxParm为准）|
|loadFilter   |function    |是  |null |从后端获取值后做特殊处理（该函数接受一个参数data，即为从后端ajax返回的值）|



**该插件可供选择的事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onLoadSuccess   |data    |菜单加载完成之后执行（参数data即为ajax或直接传入的dataList）|
|onClickMenu   |item,parent    |点击菜单的时候执行|



**该插件暂未提供可供选择的方法（method）：**


----------


## 2、显示一个警告框（alerts）##

显示一个导航菜单

依赖于：无
继承于：无

使用举例
使用举例
```html
<div id="div"></div>
```

```javascript
    $("#div").alerts({
	    content : "内容",
		showOnCreate : false
	});
```
	
**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.alerts.js

**该插件可供选择的属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|content   |string    |是  | "" |提示框显示内容|
|showOnCreate   |boolean    |是  |true |是否在创建完就显示|
|type   |string    |是  |"info" |显示框类型（取值见枚举定义2）|



**该插件可供选择的事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onBeforeShow   |none    |显示之前执行（当该函数返回false时，则会阻止显示）|
|onAfterShow   |none    |显示之后执行|
|onBeforeHide   |none    |隐藏之前执行（当该函数返回false时，则会阻止隐藏）|
|onAfterHide   |none    |隐藏之后执行|



**该插件暂未提供可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 | 
| ---------|:--------:| --------:| 
|hide   |speed    |隐藏提示框，参数为jquery的hide参数| 
|show   |speed    |显示提示框，参数为jquery的show参数| 



----------


## 3、显示一个面包屑导航（breadcrumb）##

显示一个面包屑导航

依赖于：无
继承于：无

使用举例
使用举例
```html
<div id="div"></div>
```
```javascript
    $("#div").breadcrumb({
		data: [{
            text: "CRM首页",
            href: "http://www.baidu.com"
        }, {
            text: "会员数据"
        }, {
            text: "会员信息"
        }]
	});
```


**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.breadcrumb.js

**该插件可供选择的属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|textField   |string    |是  | "text" |data中显示文字的字段名|
|hrefField   |string    |是  |"href" |data中链接地址的字段名|
|data   |array    |否  |[] |面包屑导航的值|



**该插件暂未有供选择的事件（event）有：**




**该插件暂未提供可供选择的方法（method）：**  



----------


## 4、显示一个下组合框（combo）##

显示一个组合框

依赖于：panel
继承于：textbox

使用举例
使用举例
```html
<input type="text" id="txt" />
```
```javascript
    $("#txt").combo({
	    panelSelector : "#selector",
		panelHeight : 200,
		panelWidth : 300
	});
```
	

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.combo.js

**该插件属性完全继承与textbox的所有属性，另外可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|panelSelector   |string&#124;object    |否  | undefined |组合框的下拉面板的选择器或jquery对象|
|panelHeight   |string&#124;int    |是  |"auto" |下拉面板的高度|
|panelWidth   |string&#124;int    |是  |undefined |下拉面板的宽度（默认与输入框一样）|



**该插件事件完全继承与textbox，另外可供选择的私有事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onBeforeShowPanel   |none    |显示之前执行（当该函数返回false时，则会阻止显示）|
|onAfterShowPanel   |none    |显示之后执行|
|onBeforeHidePanel   |none    |隐藏之前执行（当该函数返回false时，则会阻止隐藏）|
|onAfterHidePanel   |none    |隐藏之后执行|



**该插件方法完全继承与textbox，另外提供的私有可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|getPanel   |none    |获取下拉面板的jquery对象|
|hidePanel   |none    |隐藏面板|
|showPanel   |none    |显示面板|


----------


## 5、显示一个下组合下拉框（combobox）##

显示一个组合下拉框

依赖于：panel
继承于：combo

使用举例
```html
<input type="text" id="txt" />
```

```javascript
    $("#txt").combobox({
	    url : "xxxx.do",
	    queryParams : {
	        id : 1,
	        name : "name"
	    },
		panelHeight : 200,
		panelWidth : 300
	});
```
	

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.combobox.js

**该插件属性完全继承与combo的所有属性，另外可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|valueField   |string    |是  | "value" |数据中value值的属性名|
|textField   |string    |是  |"text" |数据中text值的属性名|
|className   |string    |是  |undefined |组合框的样式|
|value   |string    |是  |null |默认值|
|url   |string    |是  |null |从远端获取数据的url|
|method   |string    |是  |"POST" |从远端获取数据时的方式（POST/GET）|
|queryParams   |object    |是  |{} |从远端获取数据时，传递的额外参数|
|dataType   |string    |是  |"json" |从远端获取数据时，返回数据的格式|
|data   |arrar    |是  |[] |需要加载的数据（url和data同时有值时，url优先）|
|formatter   |function(rowData, rowIndex)    |是  |null |显示每一行数据时，需要做的特殊处理（需要返回一个对象，包含value和text属性）|
|loadFilter   |function(data)    |是  |null |从远端获取数据后，进行一些处理|
|maxlength   |number    |是  |0 |最多显示多少行数据。为0，则全部显示|
|forceRemote   |boolean    |是  |false |是否强制从远端获取数据（为true，则每次搜索都要从远端获取数据）|



**该插件事件完全继承与combo，另外可供选择的私有事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onChange   |value, rowData    |当选择一行触发|
|onLoadSuccess   |data    |当从后端加载完数据后触发|




**该插件方法完全继承与combo，另外提供的私有可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|getValue   |none    |获取值|
|setValue   |value    |设置值|
|getPanel   |none    |获取panle|
|getListPanel   |none    |获取下拉列表框|


----------


## 6、显示一个下组合下拉表格（combotable）##

显示一个组合下拉表格

依赖于：panel,table
继承于：combo

使用举例
```html
<input type="text" id="txt" />
```
```javascript
    $("#txt").combotable({
	    table : {
	        url : "xxxx.do",
    	    columns : [{
				checkbox : true
			}, {
				title : "id",
				field : "id"
			}, {
				title : "name",
				field : "name"
			}]
	    },
		panelHeight : 200,
		panelWidth : 300
	});
```
	
**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.combotable.js


**该插件属性完全继承与combo的所有属性，另外可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|valueField   |string    |是  | "value" |数据中value值的属性名|
|textField   |string    |是  |"text" |数据中text值的属性名|
|className   |string    |是  |undefined |组合框的样式|
|value   |string    |是  |null |默认值|
|table   |object    |否  |{} |table插件的全部属性|
|multiple   |boolean    |是  |false |是否可以多选|




**该插件事件完全继承与combo，另外可供选择的私有事件（event）有：**




**该插件方法完全继承与combo，另外提供的私有可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|getValue   |none    |获取值|
|setValue   |value    |设置值|
|getTable   |none    |获取表格的对象|
|getText   |none    |获取选中的文本|



----------


## 7、显示一个模态对话框（dialog）##

显示一个模态对话框

依赖于：无
继承于：无

使用举例
```html
<div id="dialog"></div>
```
```javascript
    $("#dialog").dialog({
	    href : "xxxx.do",
	    queryParams : {
	        id : 1,
	        name : "name"
	    }
	});
```
	

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.dialog.js

**可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|height   |string&#124;int    |是  | null |对话框的高度|
|width   |string&#124;int    |是  | null |对话框的宽度|
|size   |string    |是  | "" |尺寸，有modal-sm或modal-lg|
|style   |object    |是  | {} |对话框样式|
|animate   |boolean    |是  | true |是否显示动画效果|
|tabIndex   |number    |是  | -1 |对话框在页面上的层级高度|
|closeAble   |boolean    |是  | true |是否显示关闭图标|
|title   |string    |是  | "" |对话框标题|
|content   |string    |是  | undefined |对话框内容|
|href   |string    |是  | "" |从远端加载页面内容的地址|
|queryParams   |object    |是  | {} |从远端加载页面时传递的参数|
|backdrop   |boolean&#124;string    |是  | true |如果为 static 则点击空白不会关闭对话框|
|keyboard   |boolean    |是  | true |是否可以使用esc键关闭|
|show   |boolean    |是  | true |是否显示对话框|
|buttons   |array    |是  | [] |对话框按钮|

其中，buttons属性为一数组，数组中包含的属性有

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|className   |string    |是  | undefined |按钮样式|
|text   |string    |是  | undefined |按钮文字|
|onclick   |function    |是  | undefined |按钮点击事件|
|disabled   |boolean    |是  | undefined |是否禁用|
|id   |string    |是  | undefined |按钮id|






**可供选择的私有事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onBeforeShow   |none    |显示之前触发|
|onAfterShow   |none    |显示之后触发|
|onBeforeHide   |none    |隐藏之前触发|
|onAfterHide   |none    |隐藏之后触发|
|onBeforeLoad   |none    |从远端加载数据之前触发|
|onLoadSuccess   |data    |当从后端加载完数据后触发|




**提供的私有可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|show   |none    |显示对话框|
|hide   |none    |隐藏对话框|
|toggle   |none    |显示或隐藏对话框|
|title   |title    |设置或获取对话框的标题|
|content   |content    |设置或获取对话框的内容|
|destory   |none    |销毁对话框|


----------


## 8、显示一个输入表单（form）##

显示一个输入表单
利用该插件，可以实现ajax无刷新的上传文件功能

依赖于：无
继承于：无

使用举例
```html
<form id="f1" name="f1"></form>
```
```javascript
    //初始化form
    $("#f1").form({
	    loadUrl : "xxxx.do",
	    loadParams : {
	        id : 1,
	        name : "name"
	    }
	});
	/*初始化form后，会自动把从loadUrl后端返回的数据，按照对象名称与form里面的表单的name自动装载数据。
	例如：通过loadUrl返回的
	data={
	    userName:"admin",
	    userId:1
	};
	这个时候，form表单会自动把form里面name=userName的输入框赋值为admin，name=userId的输入框赋值为1
	例如：返回的data={
	    userId:1
	}
	如果是文本框，则自动把name=userId 的输入框赋值为1
	如果是单选框，则自动把name=userId，并且value=1的单选框选中
	如果是下拉框，则自动选中name=userId的下拉值
	如果是多选框，则value值可以是string，也可以是数组
	    如果是string，数值可以以逗号分隔
	    会自动选中所有name=userId，并且value值为数组中每一个选项或string中逗号中每一个元素。
	*/
	
	//提交表单
	//调用submit方法时，会自动把form里面的所有表单信息提交到后台
	$("#f1").form("submit", {
	    submitUrl : "xxx.do",
	    success : function(data) {
	        //回调执行
	    }
	});
```
	

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.form.js

**可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|loadDepend   |array    |是  | [] |初始化form的时候，依赖哪些插件完成|
|id   |string    |是  | "" |form的id|
|name   |string    |是  | "" |form的name|
|submitUrl   |string    |是  | "" |提交form时的url|
|submitData   |object    |是  | {} |提交form时额外传递的参数|
|method   |string    |是  | "POST" |提交或初始化form的时候，使用的ajax提交方式|
|dataType   |string    |是  | "json" |提交或初始化form的时候，后端返回的数据格式|
|hasFile   |boolean    |是  | false |form中是否包含文件上传（如果为true，则会自动创建一个iframe进行无刷新的提交）|
|loadData   |object    |是  | {} |初始化form的对象|
|loadUrl   |string    |是  | "" |初始化form时，请求的url|
|loadParams   |string    |是  | "" |初始化form时，额外传递的参数|
|loadFilter   |function(data)    |是  | return data; |初始化form时，对后端返回的值进行特殊处理|



**可供选择的私有事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onBeforeSubmit   |none    |提交前执行，如果返回false，则阻止提交|
|success   |data    |提交完后，的回调函数|
|onBeforeLoad   |loadParams    |加载数据前执行，如果返回false，则停止加载数据|
|onLoadSuccess   |data    |form加载完成之后执行|




**提供的私有可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|submit   |submitParam    |提交form，参数为提交时传递的额外参数|
|validate   |none    |验证form中所有的加了validate插件的表单，全部通过，则返回true，只要有一个验证不通过，则返回false|
|load   |data    |根据指定的值，加载form|
|reset   |none    |清空form|



----------


## 9、显示一个菜单（menu）##

显示一个可以多层级的菜单

依赖于：无
继承于：无

使用举例
```html
<div id="menu"></div>
```
```javascript
    
    $("#menu").menu({
	    url : "xxxx.do"
	});
	
	//菜单中，url返回值或data的格式为
	[
	    {id:1,pid:0,text:"菜单1"},
	    {id:2,pid:0,text:"菜单2"},
	    {id:3,pid:0,text:"菜单3"},
	    {id:4,pid:1,text:"菜单4"},
	    {id:5,pid:1,text:"菜单5"},
	    {id:6,pid:2,text:"菜单6"},
	    {id:7,pid:2,text:"菜单7"}
	]
	//也可以用idField，pidField，textField来指定data中对应的属性名
```
	

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.menu.js

**可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|buttonClass   |string    |是  | "btn-primary" |菜单按钮的样式。取值参见枚举定义3|
|buttonName   |string    |是  | "下拉菜单" |下拉菜单的文本|
|rootPid   |number    |是  | 0 |菜单中根节点的pid值|
|idField   |string    |是  | "id" |data中的id属性名称|
|pidField   |string    |是  | "pid" |data中的pid属性名称|
|textField   |string    |是  | "text" |data中的text属性名称|
|url   |string    |是  | "" |到后端请求的url地址|
|queryParams   |object    |是  | {} |提交到后端请求额外参数|
|dataType   |string    |是  | "json" |到后端请求的url后返回数据的格式|
|type   |string    |是  | "POST" |到后端请求的url时的提交方式|
|data   |array    |是  | [] |直接使用该值进行加载菜单|
|loadFilter   |function(data)    |是  |  |请求|





**可供选择的私有事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onLoadSuccess   |data    |加载完成之后执行|
|onClick   |item    |点击菜单后执行（item为该菜单的值对象）|




**提供的私有可供选择的方法（method）：**  




----------


## 10、显示一个分页插件（pagination）##

显示一个分页插件

依赖于：无
继承于：无

使用举例
```html
<div id="page"></div>
```
```javascript
    
    $("#page").pagination({
	    totalCount : 100,
	    pageSize : 10
	});
	
```
	

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.pagination.js

**可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|totalCount   |int    |是  | 0 |总条数|
|pageSize   |int    |是  | 10 |每页条数|
|currentPage   |int    |是  | 1 |当前页数|
|previousPageText   |String    |是  | "&laquo;" |上一页的文字|
|nextPageText   |String    |是  | "&raquo;" |下一页的文字|





**可供选择的私有事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onSelectPage   |currentPage, pageSize    |当选择一页时执行|





**提供的私有可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|selectPage   |pageNum    |选择一页|
|getPagination   |none    |获取分页对象|


----------


## 11、显示一个面板插件（panel）##

显示一个面板插件

依赖于：无
继承于：无

使用举例
```html
<div id="panel"></div>
```
```javascript
    
    $("#panel").panel({
	    title : "面板标题",
	    content : "面板内容"
	});
	
```
	

**代码地址**

http://git.yoho.cn/yoho30/yohobuy-crm/blob/master/web/src/main/webapp/js/bootstrap-plugin/bootstrap.panel.js

**可供选择的私有属性（option）有：**

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|className   |string    |是  | "panel-default" |面板样式（取值见：面板样式枚举1）|
|style   |object    |是  | {} |样式|
|headClassName   |string    |  | "" |头部样式css|
|headStyle   |object    |是  | {} |头部样式|
|headTitleSize   |int    |是  | 3 |面板标题的文字大小（就是1,2,3）|
|bodyClassName   |string    |是  | "" |面板body  css|
|bodyStyle   |object    |是  | {} |面板body样式|
|footClassName   |string    |是  | "" |面板底部css|
|footStyle   |object    |是  | {} |面板底部样式|
|title   |string    |是  | "" |标题|
|selector   |string    |是  | undefined |加载到panel内容的选择器(优先级selector>content>url)|
|content   |string&#124;object    |是  | "" |内容|
|url   |string    |是  | "" |从远端加载内容|
|queryParams   |object    |是  | {} |从远端加载内容传递的参数|
|buttons   |array    |是  | [] |底部的按钮|
|showFooter   |boolean    |是  | false |是否显示底部|


其中button包含属性

| 属性名称 | 属性类型  | 可否为空 | 默认值  | 备注 |
| ---------|:--------:| --------:|-----:|------:|-----:|
|className   |string    |是  | null |按钮样式|
|text   |string    |是  | "" |按钮文本|
|disabled   |boolean    |是  | false |是否禁用按钮|
|id   |string    |是  | null |按钮id|
|onclick   |function    |是  | null |点击事件|



**可供选择的私有事件（event）有：**

| 事件名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|onLoadSuccess   |data    |当加载完成时执行|





**提供的私有可供选择的方法（method）：**  

| 方法名称 | 参数 | 备注 |
| ---------|:--------:| --------:|
|hide   |effect    |隐藏面板（参数为效果）|
|show   |effect    |显示面板（参数为效果）|
|setTitle   |title    |设置标题|
|getTitle   |none    |获取标题|
|setContent   |content    |设置内容|
|getContent   |none    |获取内容|








     
     
     