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
而且由于对象创建好了,所以反应速度快,(推荐使用).
但是单例模式还有一种懒汉式
``` bash
package singleton;
public class Sun {
	private static Sun sun;
	private Sun(){	
	}
	public static Sun getInstance(){
		if(sun==null){
			sun=new Sun();
		}
		return sun;
	}
	public void light(){
		System.out.println("太阳会发光,阳光普照");
	}
}
```
懒汉式就是开始不自行实例化,当我们需要的时候,先判断是否为空,为空才实例化,不为空直接返回.但是在多线程情况下,这是有线程安全问题的,比如当Sun还没初始化的时候,几个线程同时调用getInstance()方法,给Sun初始化,造成内存中不止一个Sun对象,这就可能使单例失败.最简单的方法是在getInstance()方法上加一个synchronized修饰符,如下:
``` bash
	public static synchronized Sun getInstance(){
		if(sun==null){
			sun=new Sun();
		}
		return sun;
	}
```
当一个线程进入getInstance()时,就将此方法锁住,别的线程无法进来,就无法造成同时初始化的情况.但是,安全是安全了,由于方法被锁住,每次访问这个方法的时候,就只能让一个进入,另外的在外面等待,就带来的性能影响。所以,这种是并不可取的,所以我们不是还有一种锁住的方法吗,如下:
``` bash
	public static Sun getInstance(){
		if(sun==null){
			//1
			synchronized(Sun.class){
				sun=new Sun();
			}
		}		
		return sun;
	}
```
这样,我们只锁住初始化的这段,貌似的样子(只有当第一次实例化的时候锁住,防止了同时实例化,当sun已经存在的时候,就可以随时进来拿了).但是不知你想过没得,这种情况也有问题,如果刚开始没初始化,AB都为空,都进去我在代码中标注的1里面,于是A再进入锁住的内容中,初始化一次,A完过后,B再进入其中,再次初始化一次,这就造成了单例失败,所以我们可以这样:
``` bash
	public static  Sun getInstance(){
		if(sun==null){
			synchronized(Sun.class){
				if(sun==null){
					sun=new Sun();
				}
			}
		}	
		return sun;
	}
```
双重检查,由于当A出来的时候,已经存在sun了,所以B就不会通过第二重检查,就不会再次new了,而且,以后进入的时候,由于sun!=null(没锁就不会等待),就直接返回sun,是不是就解决了性能问题,这就是双重检查的懒汉式.但是懒汉式还有一种静态内部类方式:
``` bash
package singleton;
public class Sun {
	private static class LazyHolder {    
	       private static final Sun sun = new Sun();    
	} 
	private Sun(){
	}
	public static  Sun getInstance(){
		return LazyHolder.sun;
	}
	public void light(){
		System.out.println("太阳会发光,阳光普照");
	}
}
```
这种也实现了线程安全,又避免了同步带来的性能影响.由于java中内部类加载时间：一般是只有运行到了才会初始化，而不是外部内加载的时候（不管是静态还是非静态内部类）,所以当我们调用getInstance()的时候,初始化内部类,顺带初始化sun,这是不是懒汉式(我们用到的时候才初始化)?但是单例模式除了懒汉和饿汉,还有一种登记式单例,但用的很少,可以忽略。

**若有不足,请批评指正**