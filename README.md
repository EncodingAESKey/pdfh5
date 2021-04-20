# pdfh5.js
[![npm version](https://img.shields.io/npm/v/pdfh5.svg)](https://www.npmjs.com/package/pdfh5) [![npm downloads](https://img.shields.io/npm/dt/pdfh5.svg)](https://www.npmjs.com/package/pdfh5)   [![MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/gjTool/pdfh5/blob/master/LICENSE) [![GitHub issues](https://img.shields.io/github/issues/gjTool/pdfh5.svg)](https://github.com/gjTool/pdfh5/issues) [![GitHub stars](https://img.shields.io/github/stars/gjTool/pdfh5.svg?style=social)](https://github.com/gjTool/pdfh5/stargazers) [![GitHub forks](https://img.shields.io/github/forks/gjTool/pdfh5.svg?style=social)](https://github.com/gjTool/pdfh5/network/members)  

**pdfh5.js 基于pdf.js和jQuery，web/h5/移动端PDF预览手势缩放插件。**

- **注意：本地绝对路径地址不能加载，跨域问题用代理或者服务端解决。** 
- **svg模式渲染存在缺陷，只能渲染普通pdf，带签名、印章的可能会渲染不全，报错，pdf.js官方目前没有给出解决方案** 
- **canvas模式本质是图片，默认进去的时候，图片在手机端被缩放，不是原分辨率，所以失真模糊。所以需要放大查看pdf** 

- [pdfh5博客主页](https://www.gjtool.cn/)  

- [pdfh5项目GitHub地址](https://github.com/gjTool/pdfh5)  



#### react、vue均可使用
#### [example/test](https://github.com/gjTool/pdfh5/tree/master/example/test)是vue使用示例，vue示例展示axios调用接口展示pdf，跨域代理，相对路径等方法
#### [example/react-test](https://github.com/gjTool/pdfh5/tree/master/example/react-test)是react使用示例

![pdfh5.js示例](https://img-blog.csdnimg.cn/20190731133403792.gif)

## 更新信息

- 2021.02.03 更新：  更新goto方法，增加base64字符码加载说明

### pdfh5在线预览 （建议使用谷歌浏览器F12手机模式打开预览）

-  [https://www.gjtool.cn/pdfh5/pdf.html?file=https://www.gjtool.cn/pdfh5/git.pdf](https://www.gjtool.cn/pdfh5/pdf.html?file=https://www.gjtool.cn/pdfh5/git.pdf)  


## 快速使用（有两种方式）

#### 一、script标签引入方式（需下载本项目文件夹css/pdfh5.css、js内所有文件）

- 	1.引入css   

```javascript
<link rel="stylesheet" href="css/pdfh5.css" />
```

- 	2.创建div  

```javascript
<div id="demo"></div>
```

- 	3.依次引入js（需引用本项目的js,不要引用官方的pdf.js,jquery可以引用其它版的）   

```javascript
<script src="js/pdf.js" type="text/javascript" charset="utf-8"></script>
<script src="js/pdf.worker.js" type="text/javascript" charset="utf-8"></script>
<script src="js/jquery-2.1.1.min.js" type="text/javascript" charset="utf-8"></script>
<script src="js/pdfh5.js" type="text/javascript" charset="utf-8"></script>
```

- 	4.实例化

```javascript
var pdfh5 = new Pdfh5('#demo', {
  pdfurl: "./default.pdf"
});
```

####  二、npm安装方式（适应于vue）, react使用方法类似vue（example/react-test是react使用示例）

- 	1.安装

```javascript
npm install pdfh5
```
- 	2.使用

```javascript
<template>
  <div id="app">
	<div id="demo"></div>
  </div>
</template>
<script>
  import Pdfh5 from "pdfh5";
  export default {
    name: 'App',
	data() {
	  return {
	    pdfh5: null
	  };
	},
	mounted() {
		//实例化
	  this.pdfh5 = new Pdfh5("#demo", {
		pdfurl: "../../static/test.pdf"
	  });
	  //监听完成事件
	  this.pdfh5.on("complete", function (status, msg, time) {
		console.log("状态：" + status + "，信息：" + msg + "，耗时：" + time + "毫秒，总页数：" + this.totalNum)
	  })
	}
  }
</script>

<style>
	@import "pdfh5/css/pdfh5.css";
	*{
	padding: 0;
	margin: 0;
	}
	html,body,#app {
	width: 100%;
	height: 100%;
	}
</style>
```

- **注意：如果css引用报错的话，按下面的方式引用。** 
```javascript
  import Pdfh5 from "pdfh5";
  import "pdfh5/css/pdfh5.css";
```

# API接口方法


## 实例化
- **pdfh5实例化的时候传两个参数，selector选择器，options配置项参数，会返回一个pdfh5实例对象，可以用来操作pdf，监听相关事件** 
```javascript
var pdfh5 = new Pdfh5(selector, options);
```
|参数名称|类型|取值|是否必须|作用|
|:---:|:---:|:---:|:---:|:---:|
|selector|  {String} | - | √ |pdfh5的容器选择器|
|options|  {Object} | - | × |pdfh5的配置项参数|

## options配置项参数列表

- **示例：** 配置项参数 pdfurl

```javascript
var pdfh5 = new Pdfh5('#demo', {
	pdfurl: "./default.pdf"
});
```

|参数名称|类型|取值|作用|
|:---:|:---:|:---:|:---:|
|pdfurl|  {String} | - |pdf地址 |
|URIenable|  {Boolean} |true、false， 默认false |  true开启地址栏file参数|
|data|  {String(blob)/Array(arraybuffer) | - |pdf文件流 ，与pdfurl二选一(二进制PDF数据。使用类型化数组（Uint8Array）可以提高内存使用率。如果PDF数据是BASE64编码的，请先使用atob（）将其转换为二进制字符串。)|
|renderType| {String}|"canvas"、"svg"，默认"canvas"|pdf渲染模式|
|pageNum| {Boolean}|true、false， 默认true|是否显示左上角页码|
|backTop| {Boolean}|true、false， 默认true|是否显示右上角回到顶部按钮|
|lazy| {Boolean}|true、false， 默认false|是否开启懒加载(实际是延迟加载图片，即屏幕滚动到pdf位置时加载图片)|
|maxZoom|  {Number}|最大倍数3|手势缩放最大倍数|
|scale|  {Number}|最大比例5，默认1.5|pdf渲染的比例|
|scrollEnable| {Boolean}|true、false， 默认true|是否允许pdf滚动|
|zoomEnable| {Boolean}|true、false， 默认true|是否允许pdf手势缩放|
|cMapUrl| {String}| 默认"https://www.gjtool.cn/cmaps/"|解析pdf时，特殊情况下显示完整字体的cmaps文件夹路径，例如 cMapUrl:"https://unpkg.com/pdfjs-dist@2.0.943/cmaps/"|
|limit| {Number}| 默认0 |限制pdf加载最大页数|
|logo| {Object}|{src:"pdfh5.png",x:10,y:10,width:40,height:40}src水印图片路径（建议使用png透明图片），width水印宽度，height水印高度，以每页pdf左上角为0点，x、y为偏移值。 默认false |给每页pdf添加水印logo（canvas模式下使用）|
|goto| {Number}| 默认0 |加载pdf跳转到第几页|
|textLayer|  {Boolean} | true、false， 默认false |是否开启textLayer，可以复制文本（canvas模式下使用）【处于测试阶段，位置偏移严重】|
|background|  {Object} | {color:"#fff",image:"url('pdfh5.png')",repeat:"no-repeat",position:"left top",size:"40px 40px"}，和css的background属性语法相同，默认false|是否开启背景图模式|

- 以下属性可选

|参数名称|类型|取值|作用|
|:---:|:---:|:---:|:---:|
|httpHeaders| {Object}| 默认空 |设置httpHeaders信息|
|withCredentials| {Boolean}| 默认false |是否使用cookie或授权标头之类的凭据发出跨站点访问|
|password| {String}| 默认空 |用于访问有密码的PDF|
|stopAtErrors| {Boolean}| 默认false |当无法成功解析关联的PDF数据时，停止解析|
|disableFontFace| {Boolean}| 默认false |默认情况下，字体会转换为OpenType字体，并通过字体规则来加载。如果禁用，字体将使用内置的字体渲染器渲染。|
|disableRange| {Boolean}| 默认false |禁用范围请求加载PDF文件。启用后，如果服务器支持部分内容请求，则将以块的形式获取PDF。|
|disableStream| {Boolean}| 默认false |禁用流式传输PDF文件数据。默认情况下，PDF.js尝试加载成块的PDF。|
|disableAutoFetch| {Boolean}| 默认false |禁用PDF文件数据的预取。启用范围请求后，即使不需要显示当前页面，PDF.js也会自动继续获取更多数据。默认值为“ false”。注意：还必须禁用流传输disableStream|

## 	pdf文件流请求示例（以jq ajax为例）
1.
```javascript
$.ajax({
	url: "https://www.gjtool.cn/pdfh5/git.pdf", //假设这是pdf文件流的请求接口
	type: "get",
	mimeType: 'text/plain; charset=x-user-defined',//jq ajax请求文件流的方式
	success: function (data) {
		var pdfh5 = new Pdfh5('#demo', {
			data: data
		});
	}
});
```
2.
```javascript
$.ajax({
	url: "https://www.gjtool.cn/pdfh5/git.pdf", //假设这是pdf文件流的请求接口
	type: "get",
	mimeType: 'text/plain; charset=x-user-defined',//jq ajax请求文件流的方式
	success: function (data) {
		var rawLength = data.length;
		var array = new Uint8Array(new ArrayBuffer(rawLength));
		for (i = 0; i < rawLength; i++) {
		    array[i] = data.charCodeAt(i) & 0xff;
		}
		var pdfh5 = new Pdfh5('#demo', {
			data: array
		});
	}
});
```
3.
```javascript
$.ajax({
	url: "https://www.gjtool.cn/pdfh5/git.pdf", //假设这是pdf文件流的请求接口
	type: "get",
	mimeType: 'text/plain; charset=x-user-defined',//jq ajax请求文件流的方式
	success: function (data) {
		var rawLength = data.length;
		var array = [];
		for (i = 0; i < rawLength; i++) {
		    array.push(data.charCodeAt(i) & 0xff);
		}
		var pdfh5 = new Pdfh5('#demo', {
			data: array
		});
	}
});
```
4. axios示例
```javascript
axios.get("https://www.gjtool.cn/pdfh5/git.pdf",{
	responseType:"arraybuffer"
}).then(res=>{
	this.pdfh5 = new Pdfh5('#demo', {
		data: res.data
	});
})
```
5. base64码加载示例
```javascript
let pdfBase64 = "adfxcvadfasdfxxxxx";

 new Pdfh5('#demo', {
 	pdfurl: "data:application/pdf;base64,"+pdfBase64,
 });
```
## methods 方法列表

- **示例：** 是否允许pdf滚动

```javascript
pdfh5.scrollEnable(true) //允许pdf滚动
pdfh5.scrollEnable(false) //不允许pdf滚动
```

|参数名称|类型|取值|作用|
|:---:|:---:|:---:|:---:|
|scrollEnable| {Boolean}|true、false， 默认true|是否允许pdf滚动(需要在pdf加载完成后使用)|
|zoomEnable| {Boolean}|true、false， 默认true|是否允许pdf手势缩放(需要在pdf加载完成后使用)|
|show| {Function}|带一个回调函数|pdfh5显示|
|hide| {Function}|带一个回调函数|pdfh5隐藏|
|reset| {Function}|带一个回调函数|pdfh5还原|
|destroy| {Function}|带一个回调函数|pdfh5销毁|
|on| {String, Function}|String：监听的事件名，Function：监听的事件回调|on方法监听所有事件|
|goto | {Number}}|Number:要跳转的pdf页数 |pdf跳转到第几页（pdf加载完成后使用）|

## on方法监听所有事件-事件名列表

- **示例：** 监听pdf准备开始渲染，此时可以拿到pdf总页数

```javascript
pdfh5.on("ready", function () {
	console.log("总页数：" + this.totalNum)
})
```
|参数名称|回调|作用|
|:---:|:---:|:---:|
|init| {Function}|监听pdfh5开始初始化|
|ready| {Function}|监听pdf准备开始渲染，此时可以拿到pdf总页数|
|error| {Function(msg,time))}|监听加载失败，msg信息，time耗时 |
|success| {Function(msg,time))}| 监听pdf渲染成功，msg信息，time耗时|
|complete| {Function(status, msg, time)}| 监听pdf加载完成事件，加载失败、渲染成功都会触发。status有两种状态success和error|
|render| {Function(currentNum, time, currentPageDom)}| 监听pdf渲染过程，currentPageDom当前加载的pdf的dom,currentNum当前加载的pdf页数,|
|zoom| {Function(scale)}| 监听pdf缩放，scale缩放比例|
|scroll| {Function(scrollTop)}| 监听pdf滚动，scrollTop滚动条高度|
|backTop| {Function}| 监听回到顶部按钮的点击事件回调|
|zoomEnable| {Function(flag)}| 监听允许缩放，flag：true，false|
|scrollEnable| {Function(flag)}| 监听允许滚动，flag：true，false|
|show | {Function}| 监听pdfh5显示|
|hide | {Function}| 监听pdfh5隐藏|
|reset | {Function}| 监听pdfh5还原|
|destroy | {Function}| 监听pdfh5销毁|
