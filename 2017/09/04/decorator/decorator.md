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

哈哈更更更不出所料,打印的结果为70 90.但不知你想过没有,如果这个women还懂礼貌呢,是不是要在women类中添加一个懂礼貌方法,是不是又要写复合类,继承StudyDressWomen类,覆盖懂礼貌这个方法,如果还有上百种优点,那这个继承体系是不是就很庞大呢,100个特性的女人继承99个特性的女人,99个特性的女人,继承98个特性的女人等等...有没得什么可以解决的呢,这个问题想一想,下次再说..