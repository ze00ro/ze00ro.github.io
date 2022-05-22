---
layout: dev
title: "问题研讨会：php 的引用/作用域问题"
categories: [article, program]
tags: 

---

php 的引用确实是个问题，尤其作用域又不太严格。这不 java 小伙子们又踩坑了。

写 java 的时候经常遍历一个数组，往里面的对象赋值，php里那么写就有些问题。

看这个代码

``` php
foreach (array(1, 2, 3) as &$row) {
    // 
}
echo $row;
```

这个 $row 的结果是3。如果下面再有循环，或有使用到 $row 就可能出问题。

所以一般在循环完后要 `unset($row)`, 最好命名都保持不一样。

---

另外再看个

``` php
$a = 1;
$b =& $a;
unset($a);
```

当 unset 一个引用，只是断开了变量名和变量内容之间的绑定。这并不意味着变量内容被销毁了。所以 $b 还是 1



