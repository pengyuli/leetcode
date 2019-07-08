# TreeMap
可以对key进行排列, 用campareTo方法从小到大排序

```
TreeMap<obj, obj> map = new TreeMap();
```
map.firstEntry();返回该map中最小的key对应的key-value,若map为空，则返回为null；

lastEntry();返回map中最大的key对应的key-value,若map为空，则返回为null；

higerEntry(Object key);返回该map中大于指定key的最小的key-value键值对，若map为空，则返回为null；

lowerEntry(Object key);返回该map中小于指定key的最大的key-value键值对，若map为空，则返回为null；

floorEntry() 返回该map中<=指定key的最大的key-value键值对，若map为空，则返回为null；

ceilingEntry() 返回该map中大于等于指定key的最大的key-value键值对，若map为空，则返回为null