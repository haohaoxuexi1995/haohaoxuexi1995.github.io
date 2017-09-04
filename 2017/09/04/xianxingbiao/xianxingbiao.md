---
title: java_线性表
categories:
- Diary
tags:
- 后端
---

线性表是最基本、最简单、也是最常用的一种数据结构。

### 线性表的定义

**线性表是最基本、最简单、也是最常用的一种数据结构。** 一个线性表是n个相同特性的数据元素的集合,线性表中的个数n定义为线性表的长度，n=0时称为**空表**.在非空表中每个数据元素都有一个确定的位置，如用ai表示数据元素，则i称为数据元素ai在线性表中的位序。线性表的相邻元素之间存在着序偶关系。如用（a1，…，ai-1，ai，ai+1，…，an）表示一个顺序表，则表中ai-1领先于ai，ai领先于ai+1，称ai-1是ai的直接前驱元素，ai+1是ai的直接后继元素。当i=1,2，…，n-1时，ai有且仅有一个直接后继，当i=2，3，…，n时，ai有且仅有一个直接前驱

### 线性表特征

	1．集合中必存在唯一的一个“第一元素”。
	2．集合中必存在唯一的一个 “最后元素” 。
	3．除最后一个元素之外，均有 唯一的后继(后件)。
	4．除第一个元素之外，均有 唯一的前驱(前件)。

此图就是一个线性表![](http://images.cnitblog.com/blog/358550/201309/11011121-def3b944aa6a4f758dd3fa2639de2203.png)

### 线性表的操作

	* printList 打印线性表
	* size 返回线性表的实际容量
	* clear 清空线性表,并将其还原成初始状态
	* isEmpty 判断线性表是否为空
	* find 返回某一项首次出现的位置
	* insert 在某个位置插入某个元素
	* remove 删除某个元素
	* findKth 返回某个位置的元素

具体代码:

``` bash
		package MyList;

		import java.util.ArrayList;
		/**
		 * 
		 * @author HeLei
		 * 
		 */
		public class MyArrayList {
			
			/**
			 * 举个例子,若房子能装100个人,现在房子里有6人,这个100就是DEFAULT_CAPACITY,6 就是 size
			 */
			private static final int DEFAULT_CAPACITY=20;  //线性表初始默认容量大小,即容纳的量
			
			private int size;   //此时线性表容量大小,即实际容量
			
			private int[] arr;   //线性表的内部实现数组
			
			/**
			 * 为数组分配容量,如果容量大于或者等于了默认容量,就重新创建一个数组,将旧数组的值线性赋值给新数组
			 * @param newCapacity
			 */
			public void distributionCapacity(int newCapacity){
				if(newCapacity < DEFAULT_CAPACITY){
					return;
				}
				
				int[] old=arr;
				arr=new int[newCapacity];
				for (int i = 0; i < size(); i++) {
					arr[i]=old[i];
				}
			}
			
			/**
			 * 清空线性表,并将其还原成初始状态,通过将此时的实际容量设置为0,以及能够容纳的量设置为20
			 */
			public void clear(){
				size=0;
				distributionCapacity(DEFAULT_CAPACITY);
			}
			
			/**
			 * 创建线性表的时候,使其实际容量设置为0,以及能够容纳的量设置为20
			 */
			public MyArrayList(){
				clear();
			}
			
			/**
			 * 返回线性表的实际容量
			 */
			public int size(){
				return size;
			}
			
			/**
			 * 打印线性表
			 */
			public void printList(){
				for (int i = 0; i < size(); i++) {
					System.out.print(arr[i]+" ");
				}
				System.out.println();
			}
			
			
			/**
			 * 在index位置插入value
			 * @param index
			 * @param value
			 */
			public void insert(int index,int value){
				if(index> size() || index<0){
					throw new IndexOutOfBoundsException();
				}
				if(size()==arr.length){
					distributionCapacity(size*2);
				}
				for (int i = size; i > index; i--) {
					arr[i]=arr[i-1];
				}
				arr[index]=value;
				size++;
			}
			
			/**
			 * 在最末尾插入value
			 * 
			 */
			public void insert(int value){
				insert(size,value);
			}
			
			/**
			 * 返回某一项首次出现的位置
			 * @param value
			 * @return
			 */
			public int find(int value){
				for (int i = 0; i < size; i++) {
					if(value==arr[i]){
						return i;
					}
				}
				return -1;
			}
			
			/**
			 * 返回某个位置的元素
			 * @param index
			 * @return
			 */
			public int findKth(int index){
				if(index>= size || index<0){
					throw new IndexOutOfBoundsException();
				}
				return arr[index];
			}
			
			/**
			 * 按值删除某个元素
			 * @param value
			 * @return
			 */
			public int remove(int value){
				if(find(value)==-1){
					return -1;
				}
				int index=find(value);
				for (int i = index; i < size-1; i++) {
					arr[i]=arr[i+1];
				}
				size--;
				return 1;
			}
			
			/**
			 * 将指定索引的值替换成我们想要的值
			 * @param index
			 * @param value
			 */
			public void replace(int index,int value){
				if(index>= size || index<0){
					throw new IndexOutOfBoundsException();
				}
				arr[index]=value;
			}
			
			/**
			 * 返回线性表是否为空
			 * @return
			 */
			public boolean isEmpty() {
				return size==0;
			}
		}
```
可以看到,线性表在查找和替换方面都是常数时间执行,而插入和删除却花费昂贵的开销,最坏的情况下,在位置0插入或者删除的话需要将整个数组后移一个空间以腾出位置或者往前依次覆盖(删除),最坏的情况为O(n),
若发生在表的最后,则没有元素需要移动,情况为O(1),平均来看,每次都需要移动表一半的数据,花费来看依然是线性时间,
所以,线性表在插入和删除方面来说并不是那么恰当的,而需要另一种数据结构--链表.

**若有不足,请批评指正**

