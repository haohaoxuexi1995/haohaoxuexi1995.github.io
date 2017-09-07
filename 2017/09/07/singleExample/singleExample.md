---
title: java_单例模式应用
categories:
- java设计模式
tags:
- 后端
---

### 介绍
各种语言都有自己所支持的配置文件类型。Java 支持的是.properties 文件
``` bash
#以下为学生信息
name=lihua
```
properties 文件。其中# 表示的是注释信息；等号“= ”左边的我们称之为key ；等号“= ”右边的我们称之为value 。（其实就是我们常说的键- 值对）key 应该是我们程序中的变量。而value 是我们根据实际情况配置的。

### 解析.properties 文件
java中有解析java.util.Properties 类,那我们现在写一个A类来解析一个名为:config.properties 文件
不了解Properties类的可以自己去查一下:
``` bash
package singleton;
import java.io.IOException;
import java.util.Properties;
public class A {
	public void getProperties(){
		//创建一个properties对象
		Properties properties=new Properties();
		try {
			//加载配置
			properties.load(A.class.getResourceAsStream("config.properties"));
			//通过配置获得属性值
			String name=properties.getProperty("name");
			System.out.println(name);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```
这时我们的B类也需要获得配置文件中的配置信息,那我们再写一个B类,和A类一样:
``` bash
package singleton;
import java.io.IOException;
import java.util.Properties;
public class B {
	public void getProperties(){
		//创建一个properties对象
		Properties properties=new Properties();
		try {
			//加载配置
			properties.load(A.class.getResourceAsStream("config.properties"));
			//通过配置获得属性值
			String name=properties.getProperty("name");
			System.out.println(name);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```
测试一下:
``` bash
package singleton;
public class Test {
	public static void main(String[] args) {
		A a=new A();
		a.getProperties();
	}
}
```
结果为:
``` bash
lihua
lihua
```
不知现在你想过没得,config.properties只有一个,但是Properties对象实例却有两个,如果我们以后C类也需要使用配置文件,那么不是还要在创建一个Properties的对象?这显然是不合理的.怎么才能让Properties的对象只有一个呢?没错,单例模式,但是,Properties是jdk的类,已经不是单例设计了,我们不能更改这个源码,那怎么办呢?我们可以写一个Config类,通过它创建唯一的Properties类对象.
``` bash
package singleton;
import java.io.IOException;
import java.util.Properties;
public class Config {
	private static Properties properties=new Properties();
	private static Config config=new Config();
	private Config(){
		try {
			properties.load(A.class.getResourceAsStream("config.properties"));
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	public static String getValue(String name){
		return properties.getProperty(name);
	}
}
```
当类加载的时候,会先加载Properties的对象(独一无二的),接着加载Config对象(独一无二的),加载的时候会调用Config的构造方法,于是将properties的以后任务完成,现在就保证了不管好多类调用,Properties实例只会存在一个,而且配置文件也是加载了的,但是注意不能讲Config的初始放在Properties初始之上,否则会报空指针异常(Config在之上的时候new的时候Properties实例还不存在,再调用load会出错),getValue方法可以获得配置文件中的属性值.以下是测试文件:
``` bash
package singleton;
public class Test {
	public static void main(String[] args) {
		String a=Config.getValue("name");
		System.out.println(a);
	}
}
```
结果:lihua,单例还有很多应用,等你慢慢去发现.

**若有不足,请批评指正**