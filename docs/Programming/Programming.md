# Language learning

## Python

列表数据类型:

```python
列表数据结构：
list.append(x):在列表末尾添加一个元素
list.extend(iterable):拼接两个列表
list.insert(i,x):
list.remove(x):删除列表中第一个指为x的元素，没有则异常
list.pop([i]):无参数默认取出最后一个元素
list.clear():
list.count():
list.sort(*,key=None,reverse=False)
list.copy()
几何数据结构：
支持成员检测(in,not in)、消除重复元素(set())以及几何运算(&,|,^,-)
在字典中循环时，用 items() 方法可同时取出键和对应的值
在序列中循环时，用 enumerate() 函数可以同时取出位置索引和对应的值
```

## C++/C

关于strlen输入的事项

```C
for (int i = 1;i <= strlen(s);i++) {
    scanf("...",...);
}
```

这样写会极大增加时间，因为没个循环都要计算一次strlen(s),当然有可能是s+1优化不了，不过以后都不应该这样写。
