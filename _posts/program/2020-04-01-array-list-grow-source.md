---
layout: dev
title: "ArrayList 的扩容机制"
categories: [article, program]
tags: 

---

ArrayList 是怎么扩容的，以下是 jdk 11 的部分源码

``` java
private int newCapacity(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity <= 0) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return minCapacity;
    }
    return (newCapacity - MAX_ARRAY_SIZE <= 0)
        ? newCapacity
        : hugeCapacity(minCapacity);
}
```

扩容大小主要是这一行

``` java
int newCapacity = oldCapacity + (oldCapacity >> 1);
```

新的容量是旧容量 + 旧容量值右移一位 得到的

右移一位相当于除以2，假如老容量是15，扩容后是 15+(15/2) = 22





