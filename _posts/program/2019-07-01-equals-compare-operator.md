---
layout: dev
title: "equals 和 == 的 比较"
categories: [article, program]
tags: 

---

## == 的作用

判断两个变量栈内存中存储的值是否相等，如果相等返回true，如果不相等返回false。

有两种形式的比较需要用到比较运算符“==”，一是两个基本数据类型之间的比较，二是两个引用数据类型之间的比较。

### 两个基本类型

Java中八大基本数据类型：byte，short，int，long，double，folat，boolean，char，其中占一个字节的是byte和boolean，short和char占两个字节，int，float占四个字节，double和long占8个字节。

这八种基本数据类型的变量在栈中存储的是其值，因此“==”比较基本数据类型时，比较的是其值。

要注意一下，JAVA 基本数据类型创建单个变量是不可以new的，因为new的都是对象。但创建基本数据类型的数组时可以new，因为数组是对象。

``` java
int a = 5;
int b = 5;
int c = 6;
System.out.println(a == b); // 5 == 5
System.out.println(a == c); // 5 == 6
```

运行结果依次为：true  false。

创建了是三个基本数据类型的变量，他们在栈中存储的值就是其数值本身，栈中值由高到底依次为a(5)、b(5)、c(6)。因此“a==b”其本质就是“5==5”，返回true；“a==c”其本质就是“5==6”，返回false。

### 两个引用数据类型

引用数据类型都是对象，一般都是在堆中被创建（还可能在常量池中被创建），变量中存储的是堆中对象的地址引用。因此，“==”比较引用类型时，比较的是地址。

一般的类用new来创建对象，是在堆中开辟空间来存储的，每个对象都有唯一的地址；

基本数据类型的包装类既可以用new来创建，在堆中开辟空间存储，也可以直接创建，通过自动装箱的机制来完成对象在堆中的创建。

这里要注意两个数据类型，一个是String类型，一个是Integer类型。

 - String 类型的创建和存储

``` java
String str1 = new String("abc");// 0x123
String str2 = new String("abc");// 0x262
String str3 = new String("xyz");// 0x762
System.out.println(str1 == str2);// 0x123 == 0x262
System.out.println(str1 == str3);// 0x123 == 0x762
```
运行结果依次为：falsefalse。

用new创建了三个String类型的对象，创建的三个对象都在堆中，有不同的地址引用。三个变量在栈中存储的是三个对象的地址引用，其值各不相同，所以返回都为false。
``` java
String str1 = "abc";// 0x111
String str2 = "abc";// 0x111
String str3 = "xyz";// 0x123
System.out.println(str1 == str2);// 0x111 == 0x111
System.out.println(str1 == str3);// 0x111 == 0x123
```
运行结果依次为：true  false。

直接创建了三个String类型的对象，创建的字符串在字符串常量池中，因此创建已经存在的字符串时会直接返回其地址引用。当创建第二个“abc”时，直接返回了字符串常量池中已有的str1创建的字符串的地址，所以str2在栈中存储的值和str1在栈中存储的值是一样的，都是字符串“abc”在字符串常量池中的地址引用，而str3在栈中存储的是字符串“xyz”在字符串常量池中的地址引用。

 - Integer 类型的创建和存储

``` java
Integer s1 = new Integer(100);
Integer s2 = new Integer(100);
Integer s3 = new Integer(200);
System.out.println(s1 == s2);
System.out.println(s1 == s3);
```
运行结果依次为：false　false。

用new创建了三个Integer类型的对象，创建的三个对象都在堆中，因此有不同的地址引用。三个变量在栈中存储的是三个对象的地址引用，所以其值各不相同。

``` java
Integer n1 = 100;
Integer n2 = 100;
System.out.println(n1 == n2);

Integer a1 = new Integer(100);
Integer a2 = new Integer(100);
System.out.println(a1 == a2);

Integer m1 = 300;
Integer m2 = 300;
System.out.println(m1 == m2);

Integer b1 = new Integer(300);
Integer b2 = new Integer(300);
System.out.println(b1 == b2);
```

运行结果依次为：true  false  false　false。

为什么是这样呢？

两个new的好解释，因为是在堆中创建的，每个对象有唯一的地址，因此会返回false。但第一个和第三个是啥情况呢？

这是因为在Java中为了提高空间和时间性能，Integer类中有一个静态内部类IntegerCache，在IntegerCache类中有一个Integer数组，用以缓存当数值范围为[-128,127]时的Integer对象。所以在自动装箱的过程中，如果数值在[-128,127]范围内，会直接返回其地址引用，若是不在其范围才在堆中创建对象。

## equals 的作用

在Java中许多系统提供的类都定义了equals方法，这个方法用来比较两个独立对象的内容是否相同，而不是比较引用值本身的。自定义方法的定义大致如下（以String类为例）：

``` java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String aString = (String)anObject;
        if (!COMPACT_STRINGS || this.coder == aString.coder) {
            return StringLatin1.equals(value, aString.value);
        }
    }
    return false;
}
```
但是，如果一个自定义的类中没有定义equals方法，那么它将继承Object类的equals方法，Object类的equals方法定义如下：

``` java
public boolean equals(Object obj) {
    return (this == obj);
}
```

可以看到，如果一个类中没有显式的定义equals方法，那么equals方法的作用和“==”的作用是一样的，都是比较两个变量指向的对象是否是同一对象。

我们来看一看下面的案例：

``` java
String str1 = new String("abc");
String str2 = new String("abc");
System.out.println(str1 == str2);
System.out.println(str1.equals(str2));
```

运行结果依次为：false   true。

“==”比较的是地址，str1和str2在堆中的地址是不一样的，因此结果返回false。因为String类型自定义了equals方法，此equals方法比较的是对象的内容，因此结果返回true。

``` java
public class Test {
    public Test() {}
    public static void main(String[] args) {
        Test a = new Test();
        Test b = new Test();
        Test c = a;
        System.out.println(a.equals(b));
        System.out.println(a.equals(c));
    }
}
```

运行结果依次是：false   true。

自己定义的的Test类，并没有自定义equals方法，因此euqals方法和“==”的作用是一样的，比较的是对象的地址，也就是说判断两个对象是否是同一个。a和b中存储的地址分别是两个不同对象的地址，c被赋值了a中存储的地址，也就是说a和c指向的是同一个对象。

## 总结

运算符“==”用来比较两个变量在栈中存放的值是否相同。基本类型变量比较的是其值，引用类型比较的是其地址，也就是看是否是同一对象。

equals是类的成员方法，一般用来比较两个独立对象的内容是否相同（要看具体的定义）。如果自定义的类中没有定义equals方法，则会继承Object类的equals方法，其作用与“==”相同。


