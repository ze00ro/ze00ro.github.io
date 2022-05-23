---
layout: dev
title: "遍历过程中操作删除的问题"
categories: [article, program]
tags: 

---

普通for循环正序删除（结果：会漏掉元素判断）

普通for循环倒序删除（结果：正确删除）

for-each循环删除（结果：抛出异常）

Iterator遍历，使用ArrayList.remove()删除元素（结果：抛出异常）

Iterator遍历，使用Iterator的remove删除元素（结果：正确删除）
