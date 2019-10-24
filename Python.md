# Python

## 基本数据类型

### String

lower()  转为小写

upper()  转为大写

len() 返回长度

### Number

1 int

1.0 float

### List


```python
list1 = ['Google', 'Runoob', 1997, 2000];
```


len() 列表元素个数

append() 在列表末尾添加新的对象

insert(index, obj) 在index位置添加元素

del list[index]  删除index位置元素

remove(obj)  删除列表中的obj

sort(key=None, reverse = Flase) 对列表以key排序

sorted(list)  返回排序后结果但是不影响原本列表

**[i:j] 显示列表元素i到j-1**

[:j] 0-> j-1

[i:] i -> 最后

### Tuple

```
tup1 = ('Google', 'Runoob', 1997, 2000);
```
tuple 不能修改

### Dictiionary

```python
d = {key1 : value1, key2 : value2 }
```

dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}

访问字典： dict['Name']

更新字典：dict['Age'] = 8
添加新的信息： dict['School'] = "Harvard"

删除信息：del dict['Name']

删除字典 del dict

### Set

```python
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}

a = set('abracadabra')
# a = {'a', 'r', 'b', 'c', 'd'}
```

set.add(obj) 添加元素

set.remove(obj) 删除元素

len(set) 计算个数



## 异常

try: except 
```Python
 try:
     f = open(arg, 'r')
except IOError:
     print('cannot open', arg)
```

