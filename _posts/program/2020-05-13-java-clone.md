---
layout: dev
title: "深拷贝，浅拷贝，引用拷贝"
categories: [article, program]
tags: 

---

关于深拷贝和浅拷贝区别，我这里先给结论：

 - 浅拷贝：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。 
 - 深拷贝：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。

上面的结论没有完全理解的话也没关系，我们来看一个具体的案例！

### 浅拷贝

浅拷贝的示例代码如下，我们这里实现了 Cloneable 接口，并重写了 clone() 方法。

clone() 方法的实现很简单，直接调用的是父类 Object 的 clone() 方法。

``` java
public class Address implements Cloneable{
    private String name;
    // 省略构造函数、Getter&Setter方法
    @Override
    public Address clone() {
        try {
            return (Address) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}

public class Person implements Cloneable {
    private Address address;
    // 省略构造函数、Getter&Setter方法
    @Override
    public Person clone() {
        try {
            Person person = (Person) super.clone();
            return person;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```

测试：

``` java
Person person1 = new Person(new Address("武汉"));
Person person1Copy = person1.clone();
// true
System.out.println(person1.getAddress() == person1Copy.getAddress());
```

从输出结构就可以看出， person1 的克隆对象和 person1 使用的仍然是同一个 Address 对象。

### 深拷贝

这里我们简单对 Person 类的 clone() 方法进行修改，连带着要把 Person 对象内部的 Address 对象一起复制。

``` java
@Override
public Person clone() {
    try {
        Person person = (Person) super.clone();
        person.setAddress(person.getAddress().clone());
        return person;
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();
    }
}
```

测试

``` java
Person person1 = new Person(new Address("武汉"));
Person person1Copy = person1.clone();
// false
System.out.println(person1.getAddress() == person1Copy.getAddress());
```

从输出结构就可以看出，虽然 person1 的克隆对象和 person1 包含的 Address 对象已经是不同的了。

那什么是引用拷贝呢？ 简单来说，引用拷贝就是两个不同的引用指向同一个对象。
