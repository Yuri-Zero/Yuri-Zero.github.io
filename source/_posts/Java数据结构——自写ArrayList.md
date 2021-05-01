---
title: Java数据结构——自写ArrayList
date: 2021-05-01 11:25:07
tags: [Java,数据结构]
cover: https://gitee.com/Zarathos/blog-image-library/raw/9c7842a329a91794bae51ea893b018f816a590c1/QQ%E5%9B%BE%E7%89%8720210403125537.jpg
coverWidth: 1319
coverHeight: 742
categories: 数据结构与算法
---
在Java中有一个集合(C++中的容器)——ArrayList，效果类似于C++中的std::vector，是一种自动扩容数组。<br/>
这里我自己写了一个，这篇文章留作备份纪念，在这篇文章的代码里，它这个叫做MyListclass
<br/>
这篇文章包括:<font color="red">一个MyList接口、一个MyListclass类，以及在MyListclass类中实现的一个迭代器的内部类，MyListclass类继承了java.lang.Iterable接口以实现foreach循环支持,而内部类则主要实现Iterable的Iterator()方法本身</font>
---
注:所有代码都在Myarraylist包中，且为泛型
***
<!--more-->
<br/>
MyList接口:
```java
public interface MyList<T> {
    public void add(int idx,T data);//指定位置插入
    public boolean add(T data);//插入到最末
    public boolean IsEmpty();//判定是否为空
    public void ClearList();//清空List
    public T get(int i);//查
    public T  set(int i,T data);//改
    public T remove(int i);//删
    public int getLength();//得到当前元素数量
}
```
MyListclass类:
```java
import java.util.*;
//继承MyList接口来实现线性相关方法，继承java.lang.Iterable来实现迭代器和支持foreach
public class MyListclass<AnyType>implements MyList<AnyType>,java.lang.Iterable<AnyType> {
    private AnyType[] elems;//元素
    private int Length = 0;//初始长度
    private static final int Default_Capacity = 10;//默认扩容10

    public MyListclass() {
        doClear();
    }//构造器

    public boolean add(AnyType x) {//在最末插入指定元素
        add(getLength(), x);//实际调用在指定位置插入
        return true;
    }

    public void add(int index, AnyType x) {//指定位置插入
        if (elems.length == getLength()) {
            ensureCapacity(getLength() * 2 + 1);
        }
        for (int i = Length; i > index; i--) {
            elems[i] = elems[i - 1];
        }
        elems[index] = x;
        Length++;
    }

    public AnyType get(int idx) {//得到指定下标的元素
        if (IsEmpty() || idx >= getLength())//判定是否为空或超界
        {
            throw new ArrayIndexOutOfBoundsException();
        }
        return elems[idx];
    }

    public boolean IsEmpty() {
        return Length == 0;
    }//判定是否为空

    public void ClearList() {
        doClear();
    }

    private void doClear() {
        Length = 0;//已用长度置为0
        ensureCapacity(Default_Capacity);//初始扩容为10
    }

    public int getLength() {
        return Length;
    }//返回元素个数

    public void ensureCapacity(int newCapacity) {//扩容
        if (newCapacity < Length)//检查是否需要扩容
            return;
        AnyType[] old = elems;
        elems = (AnyType[]) new Object[newCapacity];
        for (int i = 0; i < Length; i++) {
            elems[i] = old[i];
        }
    }

    public AnyType set(int idx, AnyType data) {//在指定位置设置元素
        if (IsEmpty() || idx >= getLength()) {
            throw new ArrayIndexOutOfBoundsException();
        }
        AnyType old = elems[idx];
        elems[idx] = data;
        return old;
    }

    public AnyType remove(int idx) {//删除指定位置元素
        AnyType Removedelems = elems[idx];
        for (int i = idx; i < getLength(); i++) {
            elems[i] = elems[i + 1];
        }
        Length--;
        return Removedelems;
    }
    public Iterator<AnyType> iterator(){//内部类用来实现迭代器和支持foreach
        class iter implements Iterator<AnyType>{//继承Iterator<E>接口，重写hasNext和next方法
            private int current=0;
            public boolean hasNext(){
                return current<getLength();
            }
            public AnyType next(){
                if(!hasNext())
                    throw new java.util.NoSuchElementException();
                return elems[current++];
            }
            public void remove(){
                MyListclass.this.remove(--current);
            }
        }
        return new iter();
    }
}