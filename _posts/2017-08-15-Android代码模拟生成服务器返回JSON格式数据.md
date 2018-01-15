---
layout: post
title:  "Android代码模拟生成服务器返回JSON格式数据"
category: Android
date:   2017-08-15 20:15:48
categories: Android

---

####S：
在服务器端开发同学还没有写好接口的时候，或服务器端开发同学给出的接口不能让你满意的时候，你就可以通过以下两种方式来自己实现接口。
####T：
这里列举了两种实现方式，分别是Google的Gson解析工具包和Alibaba的FastJson解析工具包。
####A：
- 方式一：
　　Google的gson.jar中的com.google.gson.Gson.toJson(Object src)
　　在gson的api中，提供了两个重要的方法：toJson()和fromJson()方法。其中，toJson()方法用来实现将[Java](http://lib.csdn.net/base/java)对象转换为相应的JSON数据，fromJson()方法则用来实现将JSON数据转换为相应的Java对象。

    toJson()方法用于将Java对象转换为相应的JSON数据，主要有以下几种形式：
　　（1）String toJson(JsonElement jsonElement);
　　（2）String toJson(Object src);
　　（3）String toJson(Object src, Type typeOfSrc);
　　其中，
　　方法（1）用于将JsonElement对象（可以是JsonObject、JsonArray等）转换成JSON数据；
　　方法（2）用于将指定的Object对象序列化成相应的JSON数据；
　　方法（3）用于将指定的Object对象（可以包括泛型类型）序列化成相应的JSON数据。

```
public String getJsonStr() { 
    List<Person> list = new ArrayList<Person>(); 
    Person mPerson1 = new Person(01, "tom", 22);//id,name,age 
    Person mPerson2 = new Person(02, "rose", 24); 
    Person mPerson3 = new Person(03, "jack", 26); 
    list.add(mPerson1); 
    list.add(mPerson2); 
    list.add(mPerson3); 
    Gson mGson = new Gson(); 
    String jsonStr = mGson.toJson(list); 
    return jsonStr;
}
```

- 方式二：
　　Alibaba的fastjson.jar中的com.alibaba.fastjson.JSON.toJSONString(Objectobject)
　　Fastjson是一个Java语言编写的高性能功能完善的JSON库。fastjson采用独创的[算法](http://lib.csdn.net/base/datastructure)，将parse的速度提升到极致，超过所有json库，包括曾经号称最快的jackson。并且还超越了google的二进制协议protocol buf。Fastjson完全支持[http://json.org](http://json.org/)的标准，也是官方网站收录的参考实现之一。支持各种JDK类型。包括基本类型、JavaBean、Collection、Map、Enum、泛型等。支持JDK 5、JDK 6、[Android](http://lib.csdn.net/base/android)、阿里云手机等环境。
```
Map<String, Object> maps = new HashMap<String, Object>();
List<Map<String, Object>> arrayList = new ArrayList<Map<String, Object>>();　　
Map<String ,Object> params1 = new HashMap<String, Object>();　
Map<String ,Object> params2 = new HashMap<String, Object>();　
params1.put("id", 01);　　
params1.put("name", "tom");　　
params1.put("url", "http://www.baidu.com");　　
params2.put("id", 02);　　
params2.put("name", "jack");　　
params2.put("url", "http://www.google.com");　　
arrayList.add(params1);　　
arrayList.add(params2);　　
maps.put("desc", "json");　　
maps.put("age", "29");　　
maps.put("users", arrayList);　　
String jsonStr = JSON.toJSONString(maps);
```
####R：
这里偷个懒，就不图了，使用Log打印一下jsonStr就可以看到json格式的数据了。。

　　　　ok!两种实现方式如上，仅供学习。
