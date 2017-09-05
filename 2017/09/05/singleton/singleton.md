---
title: java_单例模式
categories:
- java设计模式
tags:
- 后端
---
### 定义
单例模式，是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例的特殊类。
通过单例模式可以保证系统中一个类只有一个实例。即一个类只有一个对象实例

### 使用场景
	< 引用别人的总结

	适用场景： 
	    单例模式只允许创建一个对象，因此节省内存，加快对象访问速度，因此对象需要被公用的场合适合使用，如多个模块使用同一个数据源连接对象等等。如： 
	    1.需要频繁实例化然后销毁的对象。 
	    2.创建对象时耗时过多或者耗资源过多，但又经常用到的对象。 
	    3.有状态的工具类对象。 
	    4.频繁访问数据库或文件的对象。 
	以下都是单例模式的经典使用场景： 
	    1.资源共享的情况下，避免由于资源操作时导致的性能或损耗等。如上述中的日志文件，应用配置。 
	    2.控制资源的情况下，方便资源之间的互相通信。如线程池等。 
	应用场景举例： 
	    1.外部资源：每台计算机有若干个打印机，但只能有一个PrinterSpooler，以避免两个打印作业同时输出到打印机。内部资源：大多数软件都有一个（或多个）属性文件存放系统配置，这样的系统应该有一个对象管理这些属性文件 
	    2. Windows的Task Manager（任务管理器）就是很典型的单例模式（这个很熟悉吧），想想看，是不是呢，你能打开两个windows task manager吗？ 不信你自己试试看哦~ 
	    3. windows的Recycle Bin（回收站）也是典型的单例应用。在整个系统运行过程中，回收站一直维护着仅有的一个实例。 
	    4. 网站的计数器，一般也是采用单例模式实现，否则难以同步。 
	    5. 应用程序的日志应用，一般都何用单例模式实现，这一般是由于共享的日志文件一直处于打开状态，因为只能有一个实例去操作，否则内容不好追加。 
	    6. Web应用的配置对象的读取，一般也应用单例模式，这个是由于配置文件是共享的资源。 
	    7. 数据库连接池的设计一般也是采用单例模式，因为数据库连接是一种数据库资源。数据库软件系统中使用数据库连接池，主要是节省打开或者关闭数据库连接所引起的效率损耗，这种效率上的损耗还是非常昂贵的，因为何用单例模式来维护，就可以大大降低这种损耗。 
	    8. 多线程的线程池的设计一般也是采用单例模式，这是由于线程池要方便对池中的线程进行控制。 
	    9. 操作系统的文件系统，也是大的单例模式实现的具体例子，一个操作系统只能有一个文件系统。 
	    10. HttpApplication 也是单位例的典型应用。熟悉ASP.Net(IIS)的整个请求生命周期的人应该知道HttpApplication也是单例模式，所有的HttpModule都共享一个HttpApplication实例.

### 代码实现
	
	开始单例之前我们来看一个问题:下面是一个太阳类
``` bash
package singleton;
public class Sun {
	public void light(){
		System.out.println("太阳会发光,阳光普照");
	}
}
```
我们再进行一个测试:
``` bash
package singleton;
public class Test {
	public static void main(String[] args) {
		Sun sun1=new Sun();
		System.out.println(sun1);
		Sun sun2=new Sun();
		System.out.println(sun2);
	}
}
```
结果为: 		singleton.Sun@7f39ebdb
			singleton.Sun@33abb81e
通过结果可以看出,Sun1 和 sun2 不是同一个对象,但是太阳不是只有唯一一个???所以为了弥补这个问题,我们就需要设计一个类,让其只能产生一个唯一的太阳,既然是一个唯一的,所以我们就不能让用户随便的调用构造方法(用户调用构造方法可以无限的创建对象),于是我们将构造方法私有了.其次,没有了构造方法,用户就不能通过类创建对象进而使用类了,怎么办呢?那我们就应该提供一个方法,这个方法可以为用户提供一个我们在类中已经提供好的对象,所以代码如下:
``` bash
package singleton;
public class Sun {
	//static 是因为getInstance()是static的,
	//随着类的加载而加载，即使此类还未new过对象，这个类变量也存在，而且仅一份。
	private static Sun sun=new Sun();
	private Sun(){
		
	}
	//static 是因为static之后这个方法就不属于对象了,可通过类名调用
	public static Sun getInstance(){
		return sun;
	}
	public void light(){
		System.out.println("太阳会发光,阳光普照");
	}
}
```
通过类名.getInstance(),我们可以获得Sun的一个对象,由于这是个静态变量,所以随着类的加载而加载,而且仅一份。保证了是同一个对象.
``` bash
package singleton;
public class Test {
	public static void main(String[] args) {
		Sun sun1=Sun.getInstance();
		System.out.println(sun1);
		Sun sun2=Sun.getInstance();
		System.out.println(sun2);
	}
}
```
结果: 	singleton.Sun@7f39ebdb
		singleton.Sun@7f39ebdb
好,现在是一个太阳了...这种创建单例的写法我们也叫做饿汉式,为什么?因为在类初始化时，已经自行实例化(感觉很饿的样子),不管你的对象是否为空.饿汉式在类创建的同时就已经创建好一个静态的对象供系统使用,以后不再改变,所以天生是线程安全的.
而且由于对象创建好了,所以反应速度快,(推荐使用),懒汉式下次再说...