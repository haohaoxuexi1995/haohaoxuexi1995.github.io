---
title: java_装饰者模式
categories:
- java设计模式
tags:
- 后端
---
在某些情况下,你需要对象的行为发生一些细小的变化,并且这些变化可以进行组合,这时你就需要**装饰者模式**.装饰者模式是在不必改变原类文件和使用继承的情况下，动态地扩展一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象。

### 通俗的理解

	比如说我们修房子,当房子还造好的时候,还只是很空旷的一片,这是我们需要门,我们就可以给它装饰一个门,需要床,就可以装饰一间床,需要窗帘的时候,我们又可以买窗帘,如果此时我们不需要门了,我们又可以将门去下,等待再次被需要的时候.

###  举个不恰当的小例子

	如果现在有一个women,她有智商和颜值,并且我们可以获得她的智商和颜值.那么她的实现类应该为:

``` bash
package decorator;
public class Women {
	private int beauty; // 一个女人的颜值
	private int iq; // 一个女人的IQ
	public Women(int beauty, int iq) {
		this.beauty = beauty;
		this.iq = iq;
	}
	public int getBeauty() {
		return beauty;
	}
	public int getIq() {
		return iq;
	}
}
```	

此时的我们可以测试一下这个类:

``` bash
package decorator;
public class Test {
	public static void main(String[] args) {
		Women fengjie=new Women(50, 70);
		System.out.println(fengjie.getBeauty()+" "+fengjie.getIq());
	}
}
```

不出所料,打印的结果为50 70.但是如果我们让这个women去读书呢?读书不是可以增加IQ呢.于是我们创建一个StudyWomen类

``` bash
package decorator;
public class StudyWomen extends Women{
	/**
	 * StudyWomen的构造方法
	 * @param beauty
	 * @param iq
	 */
	public StudyWomen(int beauty, int iq) {
		super(beauty, iq);
	}
	/**
	 *由于是学习过后的women,所以颜值不变
	 * 
	 */
	public int getBeauty() {
		return super.getBeauty();
	}
	/**
	 *在父类的基础上将IQ增加20
	 * 
	 */
	public int getIq() {
		return super.getIq()+20;
	}
}
```

此时的我们可以测试一下这个类:

``` bash
package decorator;
public class Test {
	public static void main(String[] args) {
		StudyWomen fengjie=new StudyWomen(50, 70);
		System.out.println(fengjie.getBeauty()+" "+fengjie.getIq());
	}
}
```

哈哈更不出所料,打印的结果为50 90.但是这个women不喜欢读书,喜欢打扮呢?那IQ就不会增加了,相反增加的是颜值,于是我们创建一个DressWomen类

``` bash
package decorator;
public class DressWomen extends Women{
	public DressWomen(int beauty, int iq) {
		super(beauty, iq);
	}
	public int getBeauty() {
		return super.getBeauty()+20;
	}
	public int getIq() {
		return super.getIq();
	}
}
```

此时的我们可以测试一下这个类:

``` bash
package decorator;
public class Test {
	public static void main(String[] args) {
		DressWomen fengjie=new DressWomen(50, 70);
		System.out.println(fengjie.getBeauty()+" "+fengjie.getIq());
	}
}
```

哈哈更更不出所料,打印的结果为70 70.但是,你想过没有,如果这个women又喜欢读书,而且喜欢打扮呢?那么,我们现在就需要继承StudyWomen或者DressWomen中的其中一个,重写另一个方法,那我继承DressWomen

``` bash
package decorator;
public class StudyDressWomen extends DressWomen{
	public StudyDressWomen(int beauty, int iq) {
		super(beauty, iq);
	}
	public int getIq() {
		return super.getIq()+20;
	}
}
```

哈哈更更更不出所料,打印的结果为70 90.但不知你想过没有,如果这个women还懂礼貌呢,是不是要在women类中添加一个懂礼貌方法,是不是又要写复合类,继承StudyDressWomen类,覆盖懂礼貌这个方法,如果还有上百种优点,那这个继承体系是不是就很庞大呢,100个特性的女人继承99个特性的女人,99个特性的女人,继承98个特性的女人等等...有没得什么可以解决的呢,这个问题想一想,下面再说..

现在,我们既然知道了继承体系的问题,那么,用什么解决呢?我们还是可以创建一个新的Women类

``` bash
package decorator2;
public class Women {
	private int beauty; // 一个女人的颜值
	private int iq; // 一个女人的IQ
	public Women(int beauty, int iq) {
		this.beauty = beauty;
		this.iq = iq;
	}
	public Women() {
	}
	public int getBeauty() {
		return beauty;
	}
	public int getIq() {
		return iq;
	}
}
```
我们把覆盖了的无参构造方法补了出来(作用后面解释),然后我们写一个化妆的女人类

``` bash
package decorator2;
public class DressWomen extends Women{
	private Women women;
	public DressWomen(Women women) {
		this.women=women;
	}
	public int getBeauty() {
		return women.getBeauty()+20;
	}
	public int getIq() {
		return women.getIq();
	}	
}
```
化妆的女人类有什么变化呢?现在,我们在化妆的女人类中增加了一个私有的Women对象,构造方法也变成了将传入的Women对象赋值给我们内部的私有Women对象,获得颜值等也从Women对象取得,这个时候父类无参构造方法的作用就体现出来了,因为化妆的女人类的构造方法中第一行会隐式的调用父类的构造方法,如果我们没写父类的无参构造方法,我们就需要在本类的第一行主动的调用父类的有参构造方法,而这行代码对我们没用,而且会使代码变得冗杂,所以我们就在父类中补上无参的构造方法,那样本类的构造方法第一行就不需要写了.

我们再写一个学习的女人类

``` bash
package decorator2;
public class StudyWomen extends Women{
	private Women women;
	public StudyWomen(Women women) {
		this.women=women;
	}
	public int getBeauty() {
		return women.getBeauty();
	}
	public int getIq() {
		return women.getIq()+20;
	}	
}
```
测试一下:
``` bash
package decorator2;
public class Test {
	public static void main(String[] args) {
		Women fengjie=new Women(50, 70);
		System.out.println(fengjie.getBeauty()+" "+fengjie.getIq());
	}
}
```
又是不出所料,打印的结果为50 70.但如果我们这样啊?
``` bash
package decorator2;
public class Test {
	public static void main(String[] args) {
		Women fengjie=new DressWomen(new Women(50, 70));
		System.out.println(fengjie.getBeauty()+" "+fengjie.getIq());
	}
}
```
这次打印的是70 70,why?当我们new出Women的时候,此时的颜值和智商分别是50与70,但是当我们将这个Women传入DressWomen中的时候,就调用了DressWomen的构造方法,将我们new的这个Women赋值给了DressWomen类中的私有的Women,并且getBeauty方法将Women的颜值加了20,由于构造方法传入的都是Women这个父类(意味着Women这个体系之下的对象都可以传入),所以如果我们需要将这个Women变成学习后的Women,我们则可以这样(链式调用):
``` bash
package decorator2;
public class Test {
	public static void main(String[] args) {
		Women fengjie=new StudyWomen(new DressWomen(new Women(50, 70)));
		System.out.println(fengjie.getBeauty()+" "+fengjie.getIq());
	}
}
```
结果是:70 90,表示我们以后如果还有一个懂礼貌的Women,那么我们只需继承Women类,进行以上的操作,然后new politWomen(new StudyWomen(new DressWomen(new Women(50, 70))))了,这些修饰类可以随意的组合,以满足你的要求,这就是装饰者模式
其实,这个装饰者模式还有很多累赘的代码,比如那个私有的Women,还有不做改变的方法等,这些公共的内容我们可以抽取出来,组成一个新类WrapperWomen,如下代码:
``` bash
package decorator2;
public class WrapperWomen extends Women{
	private Women women;
	public WrapperWomen(Women women) {
		this.women=women;
	}
	public int getBeauty() {
		return women.getBeauty();
	}
	public int getIq() {
		return women.getIq();
	}
}
```
现在,我们的DressWomen和StudyWomen都只需继承此类.这是StudyWomen的代码(因为子类会继承父类的方法和成员)
``` bash
package decorator2;
public class StudyWomen extends WrapperWomen{
	public StudyWomen(Women women) {
		super(women);
	}
	public int getIq() {
		return super.getIq()+20;
	}	
}
```
测试代码和上面一样,没有问题,现在就是一个较好的装饰者设计模式了.