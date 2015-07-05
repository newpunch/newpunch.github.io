---
layout: post
title:  "Android中JSON数据操作"
date:   2014-10-15 23:02:41
categories: Android
---

* content
{:toc}

## 介绍
最近在做一个小应用，离不开对JSON数据的操作，重点学习了一下。

>JSON：即JavaScript 对象表示法（JavaScript Object Notation）。是存储和交换文本信息的语法。

JSON数据具有如下特点：  
* JSON是轻量级的文本数据交换格式  
* 独立于语言和平台    
* JSON具有自我描述性，更易理解    

JSON与XML数据类似，比XML更小、更快、更易解析，表现如下：  
* 没有结束标签  
* 更短  
* 读写速度更快  
* 使用数组  
* 不使用保留字  

---

##JSON语法

JSON语法是JavaScript对象表示法语法的子集。主要语法有：  
* 数据在名称/值对(即键值对)中  
* 数据由逗号分隔  
* 花括号保存对象  
* 方括号保存数组  

JSON值可以是：
* 数字（整数或者浮点数）  
* 字符串（在双引号中）  
* 逻辑值（true 或者 false）  
* 数组（在方括号中）  
* 对象（在花括号中）  
* null

以下例子为JSON对象，可包含多个键值对对:  

    {"firstName":"New" , "laseName":"Punch"}

以下例子方括号内为JSON数组，可包含多个对象:

    {
    	"students":[ 
    		{"firstName":"New" , "laseName":"Punch"},
    		{"firstName":"Allen" , "laseName":"Iverson"},
    		{"firstName":"Jahn" , "laseName":"Alex"}
    	]
    }

---

##Android中操作JSON数据
读取JSON数据常用到JSONObject类和JSONArray类。  
二者API文档是：[JSONObject](http://json.org/javadoc/org/json/JSONObject.html)，[JSONArray](http://json.org/javadoc/org/json/JSONArray.html)。

JSONObject的文档中有这样一些话：

>A JSONObject is an unordered collection of name/value pairs... A JSONObject constructor can be used to convert an external form JSON text into an internal form whose values can be retrieved with the get and opt methods, or to convert values into a JSON text using the put and toString methods. A get method returns a value if one can be found, and throws an exception if one cannot be found. An opt method returns a default value instead of throwing an exception, and so is useful for obtaining optional values.

翻译如下:

>一个JSONObject是一个无序的名称/值对集合。···一个JSONObject构造函数可以把外部形式的JSON文本转换成内部形式，其值可以通过get和opt方法检索出来，或将值转换为一个JSON文本通过使用PUT和toString方法。如果一个可以被发现，get方法返回一个值，如果不能找到，则抛出一个异常。一个opt方法可以返回一个默认值，而不是抛出一个异常，这样就可以获得可选的值。

通过以下例子理解（摘自[这](http://www.cnblogs.com/xwdreamer/archive/2011/12/16/2296904.html)）：

	
	import net.sf.json.JSONArray;
	import net.sf.json.JSONObject;
	
	public class JSONTest {
	    public static void main(String args[])
	    {
	        JSONObject jsonObj0  = new JSONObject();
	        JSONObject jsonObj1  = new JSONObject();
	        JSONObject jsonObj2  = new JSONObject();
	        JSONObject jsonObj3  = new JSONObject();
	        JSONArray jsonArray = new JSONArray();
	        
	        //创建jsonObj0
	        jsonObj0.put("name0", "zhangsan");
	        jsonObj0.put("sex1", "female");
	        System.out.println("jsonObj0:"+jsonObj0);
	        
	        //创建jsonObj1
	        jsonObj1.put("name", "xuwei");
	        jsonObj1.put("sex", "male");
	        System.out.println("jsonObj1:"+jsonObj1);
	    
	        //创建jsonObj2，包含两个条目，条目内容分别为jsonObj0，jsonObj1
	        jsonObj2.put("item0", jsonObj0);
	        jsonObj2.put("item1", jsonObj1);
	        System.out.println("jsonObj2:"+jsonObj2);
	        
	        //创建jsonObj3，只有一个条目，内容为jsonObj2
	        jsonObj3.element("j3", jsonObj2);
	        System.out.println("jsonObj3:"+jsonObj3);
	    
	        //往JSONArray中添加JSONObject对象。发现JSONArray跟JSONObject的区别就是JSONArray比JSONObject多中括号[]
	        jsonArray.add(jsonObj1);
	        System.out.println("jsonArray:"+jsonArray);
	        
	        JSONObject jsonObj4  = new JSONObject();
	        jsonObj4.element("weather", jsonArray);
	        System.out.println("jsonObj4:"+jsonObj4);
	    }
	}

输出结果：  

    jsonObj0:{"name0":"zhangsan","sex1":"female"}
    jsonObj:{"name":"xuwei","sex":"male"}
    jsonObj2:{"item0":{"name0":"zhangsan","sex1":"female"},"item1":{"name":"xuwei","sex":"male"}}
    jsonObj3:{"j3":{"item0":{"name0":"zhangsan","sex1":"female"},"item1":{"name":"xuwei","sex":"male"}}}
    jsonArray:[{"name":"xuwei","sex":"male"}]
    jsonObj4:{"weather":[{"name":"xuwei","sex":"male"}]}

最后，给出一个在线JSON校验格式化工具:[http://www.sojson.com/](http://www.sojson.com/)。