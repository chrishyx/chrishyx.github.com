---
layout: post
title: "使用iframe实现download"
date: 2013-02-06 00:12
comments: true
categories: java
---
现代web项目中，下载无疑是最基本最普遍的功能之一，只是在以一些纯js框架（例如ExtJS，现在貌似叫sencha了）生成的界面中，不方便直接用超链接的方式去实现下载，那么我们可以在页面中隐藏一个iframe，然后在js中需要的地方去给iframe赋予一个url。具体实现如下：

jsp代码
```javascript
<iframe id="downloadiframe">
```

js代码（以ExtJS3为例）
```javascript
downloadiframe.location.href="../../xxxx.do";
```

java代码

```java
response.setCharacterEncoding("gb2312");//处理中文
response.reset(); 
response.setContentType("application/x-download");

String path = outfile.getAbsolutePath();
int last1 = path.lastIndexOf("\\"); 
int last2 = path.lastIndexOf("/"); 
String filedisplay = path.substring((last1 > last2 ? last1 : last2) + 1); 
filedisplay = URLEncoder.encode(filedisplay, "UTF-8"); 

response.addHeader("Content-Disposition", "attachment;filename=" + filedisplay); 
OutputStream outp = null; 
FileInputStream in = null; 
File fp = new File(path); 
try { 
	outp = response.getOutputStream(); 
	in = new FileInputStream(fp); 
	byte[] b = new byte[1024]; 
	int i = 0; 
	while ((i = in.read(b)) > 0) { 
		outp.write(b, 0, i); 
	} 
	outp.flush(); 
} finally { 
	in.close(); 
	outp.close(); 
}
```