map(映射)
=
---
首先要在C++源文件中包含``<map>``头文件
定义map变量方式如下:``map<int,string>M``(int和string的类型可变化),表示以int型为索引元素成员为string(表述可能不准确)
map函数如下:
|函数|作用|
|:-:|:-:|
|begin()         |返回指向map头部的迭代器|
|clear(）        |返回指定元素出现的次数|
|empty()         |如果map为空则返回true|
|end()           |返回指向map末尾的迭代器|
|equal_range()   |返回特殊条目的迭代器对|
|erase()         |删除一个元素|
|find()          |查找一个元素|
|get_allocator() |返回map的配置器|
|insert()        |插入元素|
|key_comp()      |返回比较元素key的函数|
|lower_bound()   |返回键值>=给定元素的第一个位置|
|max_size()      |返回可以容纳的最大元素个数|
|rbegin()        |返回一个指向map尾部的逆向迭代器|
|rend()          |返回一个指向map头部的逆向迭代器|
|size()          |返回map中元素的个数|
|swap()          |交换两个map|
|upper_bound()   |返回键值>给定元素的第一个位置|
|value_comp()    |返回比较元素value的函数|


对map的理解还有些问题,暂时仅作记录