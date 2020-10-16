# 【面试笔记】Python 篇

*摘自https://zhuanlan.zhihu.com/p/54430650*

### 1. 一行代码实现1-100之和

sum(range(1, 101))

### 2. 如何在一个函数内部修改全局变量

global 声明，不是一定要加global，要看变量的指向变没变

### 3. 列出5 个Python 标准库

os -- 操作系统相关的函数

sys -- 命令行参数

re -- 正则匹配

math -- 数值计算，提供了数学常数和数学函数

datetime -- 处理日期时间

random -- 用于生成随机数

### 4. 字典如何删除键和合并两个字典

del -- 用于删除键，例如del dict["Key"]

update -- 用于合并两个字典，例如dict1.update(dict2)，其中dict1, dict2 都必须是字典格式

### 5. 谈下Python 的GIL

GIL 是Python 的全局解释器锁，同一进程中假如有多个线程运行，一个线程在运行Python 程序的时候会**霸占Python 解释器（加了一把锁即GIL）**，使得该进程内的其他线程无法运行，等该线程运行完成后其他线程才能运行。如果线程运行过程中遇到耗时操作，则解释器锁解开，使其他线程运行。所以在多线程中，线程的运行仍是有先后顺序的，并不是同时进行的。

多进程中因为每个进程都能被系统分配资源，相当于每个进程有了一个Python 解释器，所以多进程可以实现多个进程的同时运行，缺点是进程系统资源开销大。

### 6. Python 实现列表去重的方法

先通过集合去重，再转成列表

```python
lis = [11, 12,13,12,15,16,13]
a = set(lis)
[x for x in a]
```

### 7. fun(\*args, \*\*kwargs) 中的\*args, **kwargs 是什么意思

\*args 和**kwargs 主要用于函数定义。将不定数量的参数传递给一个函数。这里的不定的意思是：预先不知道函数使用者会传递多少个参数，所以在这个场景下使用这个两个关键字。\*args 是用来发送一个非键值对的可变数量的参数列表给一个函数。

```python
def demo(args_f, *args_v):
    print args_f
    for x in args_v:
        print x
# 下面是输出测试
demo('a', 'b', 'c', 'd')
a
b
c
d
```

\*\*kwargs 允许将不定长度的键值对作为参数传递给一个函数。如果想要在一个函数里处理带名字的函数，应该使用\*\*kwargs。

```python
def demo(**args_v):
    for k, v in args_v.items():
        print k, v
# 下面是输出测试
demo(name='nfcx')
name njcx
```

### 8. Python2 和Python3 的range(100) 的区别

Python2 返回列表，Python3 返回迭代器，节约内存

### 9. 一句话解释什么样的语言能够用装饰器

函数可以作为参数传递的语言，可以使用装饰器

### 10. Python 内建数据类型有哪些

整型 -- int

布尔型 -- bool

字符串 -- str

列表 -- list

元组 -- tuple

字典 -- dict

### 11. 简述面向对象中\_\_new\_\_ 和\_\_init\_\_ 的区别

\_\_init\_\_ 是初始化方法，创建对象后，就立刻被默认调用了，函数代码块中可接收参数，如下

```python
def __init__(self, newWheelNum, newColor):
    self.wheelNum = newWheelNum
    self.color = newColor
```

1. \_\_new\_\_ 至少要有一个参数cls，代表当前类，此参数在实例化时由Python 解释器自动识别
2. \_\_new\_\_ 必须要有返回值，**返回实例化出来的实例**，这点在实现\_\_new\_\_ 时要特别注意，可以return 父类（通过super(当前类名, cls)）\_\_new\_\_ 出来的实例，或者直接是object 的\_\_new\_\_ 出来的实例
3. \_\_init\_\_ 有一个参数self，就是这个\_\_new\_\_ 返回的实例，\_\_init\_\_ 在\_\_new\_\_ 的基础上可以完成一些其他初始化动作，\_\_init\_\_ 不需要返回值
4. 如果\_\_new\_\_ 创建的是当前类的实例，会自动调用\_\_init\_\_ 函数，通过return 语句里面调用的\_\_new\_\_ 函数的第一个参数cls 来保证是当前类的实例，如果是其他类的类名，那么实际创建返回的就是其他类的实例，起始就不会调用当前类的\_\_init\_\_  函数，也不会调用其他类的\_\_init\_\_ 函数

### 12. 简述with 方法打开处理文件帮我们做了什么

打开文件在进行读写的时候可能会出现一些异常状况，如果按照常规的f.open 写法，我们需要try, except, finally ，做异常判断，并且文件最终不管遇到什么情况，都要执行finally f.close() 关闭文件，with 方法帮我们实现了finally 中的f.close()

### 13. 列表[1,2,3,4,5]，请使用map() 函数输出[1,4,9,16,25]，并使用列表推导式提取出大于10的数，最终输出[16,25]

map() 函数第一个参数是function，第二个参数一般是list，第三个参数可以写list，也可以不写，根据需求；map() 函数返回的是迭代器，需要根据题目需要转化

```python
lis = [1,2,3,4,5]
def fn(x):
    return x**2
res = map(fn, lis)
res = [i for i in res if i > 10]
# 下面是输出
res 
[16,25]
```

### 14. Python 中生成随机整数，随机小数，0-1之间小数的方法

随机整数：random.randint(a, b) 生成区间内的整数

随机小数：可以考虑使用numpy 库，利用np.random.randn(5) 生成5个随机小数

0-1 随机小数：random.random() 括号中不传参

### 15. 避免转义给字符串加哪个字母表示原始字符串

r, 表示需要原始字符串，不转义特殊字符

### 16.<div class="name"\> 中国</div\> ，用正则匹配出标签里面的内容（“中国”），其中class 的类名是不确定的

```python
import re
str = '<div class="name">中国</div>'
res = re.findall(r'<div class=".*">(.*?)</div>', str)
print(res)
# 下面是输出
['中国']
```

其中"()" 用于捕获() 中正则表达式的内容以备进一步利用处理，可以通过在左括号后面跟随"?:" 来关闭这个括号的捕获作用；将正则表达式的一部分内容进行组合，以便使用量词

### 17. Python 中断言方法举例

assert() 方法，格式如下：

```python
# assert expression
# 等价于
# if not expression:
# 	raise AssertionError
# 第二种：
# assert expression [, arguments]
# 等价于
# if not expression:
# 	raise AssertionError(arguments)
# 例如：assert 1==2, '1 不等于2'
# Traceback 的内容中包含报错信息：AssertionError: 1 不等于2
```

### 18. 数据表student 有id, name, score, city 字段，其中name 中的名字可有重复，需要消除重复行，请写sql 语句

```sql
select distinct name from student
```

### 19. 10个Linux 常用命令

ls -- 显示目录内容，类似DOS下的DIR

cat -- 显示文件内容，类似DOS的type 命令

mv -- 更改文件或者目录的名字

rm -- 删除文件，类似DOS的del 命令

mkdir -- 创建目录，类似DOS的md 命令

more -- 分屏显示输出结果

grep -- 在文件中搜素特定的字符串

find -- 搜索指定目录下的文件

file -- 判断文件的类型

chmod -- 改变文件存取权限

man -- 用于显示制定命令的描述和用法

touch -- 在系统中创建大小为0的任意类型文件

tar -- 用来压缩和解压缩文件，其中经常会用到"-cf" 和"-xf" 选项

cd -- 

pwd -- 返回当前目录的路径

cp -- 复制文件包括其子文件到指定目录

kill - 终止线程，例如kill -9 19979 // 终止线程号为19979的线程

tail -- 查看文件尾部n 行内容

head -- 查看文件头部n 行内容

vim -- 启动vi 编辑器

date -- 查看系统当前时间

### 20.  Python2 和Python3 的区别

1. Python3 使用print() 必须要以小括号包裹打印内容；Python2 既可以使用小括号的方式，也可以使用一个空格来分隔打印内容
2. Python2 range(a, b) 返回列表，Python3 中返回迭代器，节约内存
3. Python2 中使用ASCII 编码，Python3 中使用UTF-8
4. Python2 中为了正常显示中文，需要引入coding 声明，Python3 中不需要
5. Python2 中是raw_input() 函数，Python3 中是input() 函数

### 21. 列出Python 中可变数据类型和不可变数据类型，并简述原理

不可变数据类型：数值型，字符串型，和元组

不允许变量的值发生变化，如果改变了变量的值，相当于是新建了一个对象，而对于相同的值的不可变数据类型的对象，在内存中则只有一个对象（一个地址）

可变数据类型：列表list 和字典dict 

允许变量的值发生变化，即如果对变量进行append, += 等这种操作后，只是改变了变量的值，而不会新建一个对象，变量引用的对象的地址也不会发生改变，不过对于相同的值的不同对象，在内存中则会存在不同的对象，即每个对象都有自己的地址，相当于内存中对于同值的对象保存了多份，这里不存在引用计数，是实实在在的对象

### 22. s="ajldjlajfdljfddd"，去重并从小到大排序输出"adfjl"

set 去重，去重转成list，利用sort 方法排序，sort() 方法中参数reverse 赋值为False是从小到大排序

### 23. 用lambda 函数实现两个数相乘

multi = lambda a, b: a*b

print(multi(4, 5))

### 24. 字典根据键从小到大排序

这里用到了sorted() 函数关键字参数key，如下：

```python
dic = {"name":"zs", "age":18, "city":"深圳", "tel":"1362626627"}
lis = sorted(dic.items(), key = lambda i: i[0], reverse = False)
```

### 25. 利用collections 库的Counter 方法统计字符串每个单词出现的次数

### 26. 字符串a = "not 404 found 张三 99 深圳"，每个词中间是空格，用正则过滤掉英文和数字，最终输出"张三 深圳"

````python
import re
a = "not 404 found 张三 99 深圳"
lis = a.split(" ")
print(lis)
res = re.findall('\d+|[a-zA-Z]+', a)
for i in res:
    if i in lis:
        lis.remove(i)
new_str = " ".join(lis)
print(res)
print(new_str)
````

### 27. filter 方法求出列表所有奇数并构造新列表，a = [1,2,3,4,5,6,7,8,9,10]

filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件的元素组成的迭代器（需要转换成列表）。一共两个参数，第一个为函数名，第二个为序列，序列的每个元素作为参数传递给函数进行判断，然后返回True 或者False，最后将返回True 的元素放到新列表

```python
a = [1,2,3,4,5,6,7,8,9,10]
def fn(x):
    return x%2 == 1
newlist = filter(fn, a)
newlist = [i for i in newlist]
print(newlist)
```

### 28. 列表推导式求列表所有计数并构造新列表，a = [1,2,3,4,5,6,7,8,9,10]

```python
res = [i for i in a if i%2 == 1]
print(res)
```

### 29. 正则re.compile 的作用

re.compile 是将正则表达式编译成一个对象，加快速度，并重复使用

### 30. a = (1, ) b = (1), c = ("1") 分别是什么类型的数据

使用type() 函数来查看，a 为元组，b 为整型，c 为字符串

### 31. 两个列表[1,5,7,9] 和[2,2,6,8] 合并为[1,2,2,3,6,7,8,9]

extend() 函数可以将另一个集合中的元素逐一添加到列表中，区别于append 整体添加

### 32. 用Python 删除文件和用Linux 删除文件的方法

Python: os.remove(文件名)

Linux: rm 文件名

### 33. log 日志中，我们需要用时间戳记录error, warning 等的发生时间，请用datetime 模块打印当前时间戳"2018-04-01 11:38:54"

```python
# datetime 模块
import datetime
a=str(datetime.datetime.now().strftime('%Y-%m-%d %H:%M%S')) + " 星期：" + str(datetime.datetime.now().isweekday())
print(a)
```

### 34. 数据库优化查询方法

外键，索引，联合查询，选择特定字段

### 35. 描述matplotlib 开源库的基础操作

### 36. 写一段自定义异常代码

raise 自定义异常 except

### 37. 正则表达式匹配中，(.\*) 和(.\*?) 匹配区别

(.\*) 是贪婪匹配，会把满足正则的尽可能多的往后匹配

(.\*?) 是非贪婪匹配，会把满足正则的尽可能少的匹配

```python
s="<a>哈哈</a><a>呵呵</a>"
import re
res1 = re.findall("<a>(.*)</a>", s)
print("贪婪匹配", res1)
res2 = re.findall("<a>(.*?)</a>", s)
print("非贪婪匹配", res2)
# 下面是输出
贪婪匹配['哈哈</a><a>呵呵']
非贪婪匹配['哈哈', '呵呵']
```

### 38. 简述Django 的orm 

ORM，全拼Object-Relation Mapping，意为对象-关系映射

### 39. [[1,2],[3,4],[5,6]] 一行代码展开该列表，得出[1,2,3,4,5,6]

列表推导式

```python
# 展开列表
a=[[1,2],[3,4],[5,6]]
x=[j for i in a for j in i]
print(x)
# 下面是输出
[1,2,3,4,5,6]
```

也可以使用numpy 模块下的flatten() 函数用于将numpy.array 类型或者numpy.mat 类型展开成相同类型的一维对象

### 40. x="abc", y="def",z=["d","e","f"]，分别求出x.join(y) 和x.join(z) 返回的结果

join() 括号内的是可迭代对象，x 插入可迭代对象中间，形成字符串

```python
x="abc"
y="def"
z=["d","e","f"]

m=x.join(y)
n=x.join(z)
print(m)
print(n)
# 下面是输出
dabceabcf
dabceabcf
```

### 41. 举例说明异常模块中try except finally 的相关意义

try...except...else 没有捕获到异常，执行else 语句

try...except...finally 不管是否捕获到异常，都执行finally 语句

### 42. Python 中交换两个数值

```python
a, b = 3, 4
print(a, b)
a, b = b, a
print(a, b)
# 下面是输出
3 4
4 3
```

这里类似解包

### 43. 举例说明zip() 函数用法

zip() 函数在运算时，会以一个或者多个序列（可迭代对象）作为参数，返回一个元组的列表。同时将这些序列中并排的元素配对。

zip() 函数参数可以接受任何类型的序列，同时也可以有两个以上的参数；当传入参数的长度不同时，zip 能自动以最短序列长度为准进行截取，获得元组

```python
a=[1,2]
b=[3,4]
res=[i for i in zip(a,b)]
print(res)

a=(1,2)
b=(3,4)
res=[i for i in zip(a,b)]
print(res)

a="ab"
b="xyz"
res=[i for i in zip(a,b)]
print(res)
# 下面是输出
[(1,3), (2, 4)]
[(1,3), (2, 4)]
[('a', 'x'), ('b', 'y')]
```

### 44. a="张明 98分", 用re.sub，将98替换为100

```python
import re
a = "张明 98分"
ret = re.sub(r"\d+", "100", a)
print(ret)
# 下面是输出
张明 100分
```

### 45. 写5条常用sql语句

```sql
show datatables;
show tables;
desc 表名; -- 用于获取数据表结构
select * from 表名;
delete from 表名 where id = 5;
update students set gender = 0, hometown = "北京" where id = 5;
```

### 46. a="hello" 和b="你好“ 编码成bytes 类型

### 47. [1,2,3]+[4,5,6] 的结果是多少

两个列表相加，等价于extend

### 48. 提高Python 运行效率的方法

1. 使用生成器，因为可以节约大量内存
2. 循环代码优化，避免过多重复代码执行
3. 多进程，多线程，协程
4. 多个if, elif 条件判断，可以把最有可能先发生的条件放到前面，这样可以减少程序判断的次数，提高效率

### 49. 简述mysql 和redis 的区别

redis: 内存型非关系数据库，数据保存在内存中，速度快

mysql: 关系型数据库，数据保存在磁盘中，检索的话，会有一定的IO 操作，访问速度相对慢

### 50. 遇到bug 如何处理

1. 细节上的错误，通过print() 打印，能执行到print() 说明一般上面的代码都没有问题，分段检测程序是否有问题
2. 如果涉及一些第三方框架，会去查阅官方文档或者一些技术博客
3. 对于bug 的管理与归类总结，一般测试将测试出的bug 用teambin 等bug 管理工具进行记录，然后我们会一条一条进行修改，修改的过程也是理解业务逻辑和提高自己编程逻辑缜密性的方法
4. 导包问题，城市定位多音字造成的显示错误问题

### 51. 正则匹配，匹配日期2018-03-20

```python
import re
url = "https://link.zhihu.com/?target=https%3A//sycm.taobao.com/bda/tradinganaly/overview/get_summary.json%3FdateRange%3D2018-03-20%257C2018-03-20%26dateType%3Drecent1%26device%3D1%26token%3Dff25b109b%26_%3D1521595613462"
result = re.findall(r'dataRange=(.*?)%7C(.*?)&', url)
print(result)
# 下面是输出
[('2018-03-20', '2018-03-20')]
```

### 52. list=[2,3,5,4,9,6]，从小到大排序，不许用sort，输出[2,3,4,5,6,9]

涉及排序算法

这里提一个最简单无脑的，利用min() 方法求出最小值，原列表删除最小值，新列表加入最小值，递归调用获取最小值的函数，反复操作

### 53. 写一个单例模式

因为创建对象时\_\_new\_\_方法执行，并且必须return 返回实例化出来的对象，判断cls.\_\_instance是否存在，不存在的话就创建对象，存在的话就返回该对象，来保证只有一个实例对象的存在，打印ID，值一样，说明对象为同一个

```python
# 实例化一个单例
class Singleton(object):
    __instance = None
    
    def __new__(cls, age, name):
        # 如果类属性__instance 的值为None
        # 那么就创建一个对象，并且赋值为这个对象的引用，保证下次调用这个方法时
        # 能够知道之前已经创建过对象了，这样就保证了只有1 个对象
        if not cls.__instance:
            # 这里调用object 对象的最基本的__new__方法
            cls.__instance = object.__new__(cls)
            return cls.__instance
        
a = Singleton(18, "dongGe")
b = Singleton(8, "dongGe")
print(id(a))
print(id(b))

a.age = 19 # 给a 指向的对象添加一个属性
print(b.age) # 获取b 指向的对象的age 属性
# 下面是输出
2223334542920
2223334542920
19
```

### 54. 保留两位小数

题目本身只有a = "%.3f"%1.3335，让计算a 的结果，为了扩充保留小数的思路，提供round 方法

```python
# 保留两位小数
a = "%.3f"%1.3335
print(a, type(a))
b = round(float(a), 1)
print(b)
b = round(float(a), 2)
print(b)
# 下面是输出
1.333
1.3
1.33
```

### 55. 求三个方法打印结果

```python
def fn(k, v, dic={}):
    dic[k] = v
    print(dic)

fn("one", 1)
fn("two", 2)
fn("three", 3, {})
# 下面是输出
{"one": 1}
{"one": 1, "two": 2}
{"three": 3}
```

其中

*fn("one", 1)* 直接将键值对传给字典

fn("two", 2) 因为字典在内存中是可变数据类型，所以指向同一个地址，传了新的参数后，会相当于字典增加键值对

fn("three", 3, {}) 因为传了一个新字典，所以不再是原先默认参数的字典

### 56. 列出常见的状态码和意义

200 OK

请求正常处理完毕

204 No Content

请求成功处理，没有实体的主体返回

206 Partial Content

GET 范围请求已成功处理

301 Moved Permanently

永久重定向，资源已永久分配新URI

302 Found

临时重定向，期望使用GET 定向获取

304 Not Modified

发送的附带条件请求未满足

307 Temporary Redirect

临时重定向，POST不会变成GET

400 Bad Request

请求报文语法错误或者参数错误

401 Unauthorized 

需要通过HTTP 认证，或者认证失败

403 Forbidden

请求资源被拒绝

404 Not Found

无法找到请求资源（服务器无理由拒绝）

500 Internet Server Error

服务器故障或者Web 应用故障

503 Service Unavailable 

服务器超负载或者停机维护

### 57. 分别从前端，后端，数据库阐述web 项目的性能优化

##### 前端优化

1. 减少HTTP 请求，例如制作精灵图
2. HTML 和CSS 放在页面上部，javascript 放在页面下面，因为js 加载比HTML 和CSS 加载慢，所以要优先加载HTML 和CSS，以防页面显示不全，性能差，也影响用户体验

##### 后端优化

1. 缓存存储读写次数高，变化少的数据，比如网站首页的信息，商品的信息等。应用程序读取数据时，一般是先从缓存中读取，如果读取不到或者数据已失效，再访问磁盘数据库，并将数据再次写入缓存
2. 异步方式，如果有耗时操作，可以采用异步
3. 代码优化，避免循环和判断次数太多，如果多个if else 判断，优先判断最有可能发生的情况

##### 数据库优化

1. 如有条件，数据可以存放于redis，读取速度快
2. 建立索引，外键等

### 58. 使用pop 和del 删除字典中的"name" 字段，dic={"name": "zs", "age":18}

```python
dic = {"name":"zs", "age":18}
dic.pop("name")
print(dic)
dic = {"name":"zs", "age":18}
del dic["name"]
print(dic)
# 下面是输出
{'age': 18}
{'age': 18}
```

### 59. 列出常见MYSQL 数据存储引擎

**InnoDB**: 支持事务处理，支持外键，支持崩溃修复能力和并发控制。如果需要对事务的完整性要求比较高（比如银行），要求实现并发控制（比如售票），那选择InnoDB 有很大优势。如果需要频繁的更新，删除操作的数据库，也可以选择InnoDB，因为支持事务的提交和回滚。

**MyISAM**: 插入数据快，空间和内存使用比较低。如果表主要是用于插入新纪录和读出记录，那么选择MyISAM 能实现处理高效率。如果应用的完整性，并发性要求比较低，也可以使用。

**MEMORY**: 所有的数据都在内存中，数据的处理速度快，但是安全性不高。如果需要很快的读写速度，对数据的安全性要求比较低，可以选择MEMORY。它对表的大小有要求，不能建立太大的表。所以，这类数据库只使用在相对较小的数据库表。

### 60. 简述同源策略

同源策略需要同时满足以下三点要求：

1. 协议相同
2. 域名相同
3. 端口相同

http:test.com 与https:test.com 不同源 -- 协议不同

http:test.com 与http:admin.com 不同源 -- 域名不同

http:test.com 与http:test.com:8081 不同源 -- 端口不同

只要不满足其中任意一个要求，就不符合同源策略，就会出现跨域

### 61. 简述cookie 和session 的区别

1. session 在服务器端，cookie 在客户端（浏览器）
2. session 的运行依赖session id，而session id 是存在cookie 中的，也就是说，如果浏览器禁用了cookie，同时session 也会失效，存储session 时，键与cookie 中的session id 相同，值是开发人员设置的键值对信息，进行了base64 编码，过期时间由开发人员设置
3. cookie 安全性比session 差

### 62. 简述多线程，多进程

**进程**：

1. 操作系统进行资源分配和调度的基本单位，多个进程之间相互独立
2. 稳定性好，如果一个进程崩溃，不影响其他进程，但是进程消耗资源大，开启的进程数量有限制

**线程**：

1. CPU 进行资源分配和调度的基本单位，线程是进程的一部分，是比进程更小的能独立运行的基本单位，一个进程下的多个线程可以共享该进程的所有资源
2. 如果IO 操作密集，则可以多线程运行效率高，缺点是如果一个线程崩溃，都会造成进程的崩溃

**应用**：

IO 密集的用多线程，在用户输入，sleep 时，可以切换到其他线程执行，减少等待时间

CPU 密集的用多进程，因为如果IO 操作少，用多线程的话，因为线程共享一个全局解释器锁，当前运行的线程会霸占GIL， 其他线程没有GIL，就不能充分利用多核CPU 的优势

### 63. 简述any() 和all() 方法

any(): 只要迭代器中有一个元素为真就为真

all(): 迭代器中所有的判断项返回都是真，结果才为真

Python 中什么元素为假？

答案：0，空字符串，空列表，空字典，空元组，None, False

### 64. IOError, AttributeError, IndentationError, IndexError, KeyError, SyntaxError, NameError 分别代表什么异常

IOError: 输出输入异常

AttributeError: 试图访问一个对象没有的属性

IndentationError: 语法错误，代码没有正确的对齐

ImportError: 无法引入模块或者包，基本是路径问题

IndexError: 下标索引超出序列边界

KeyError: 试图访问字典中不存在的键

SyntaxError: Python 代码逻辑语法错误，不能执行

NameError: 使用一个还未赋予对象的变量

### 65. Python 中copy 和deepcopy 区别

1. 复制不可变数据类型，不管copy 还是deepcopy，都是同一个地址当浅复制的值是不可变对象时和“=”赋值情况一样，对象的id值与浅复制原来的值相同

2. 复制的值是可变对象（列表和字典）

   浅拷贝copy 有两种情况：

   第一种情况：复制的对象中无复杂子对象，原来的值的改变并不会影响浅复制的值，同时浅复制的值改变也不会影响原来的值。原来值的id 值与浅复制原来的值不同

   第二种情况：复制的对象中有复杂子对象（例如列表中的一个子元素是一个列表），改变原来的值中的复杂子对象的值影响浅复制的值

   深拷贝deepcopy：完全复制独立，包括内层列表和字典

### 66. 列出几种魔法方法并简要介绍用途

\_\_init\_\_: 对象初始化方法

\_\_new\_\_: 创建对象时执行的方法，单例模式会用到

\_\_str\_\_: 当使用print 输出对象时，只要自己定义了\_\_str\_\_(self) 方法，那么就会打印从这个方法return 的数据

\_\_del\_\_: 删除对象执行的方法

### 67. 请将[i for i in range(3)] 改成生成器

生成器是特殊的迭代器

1. 列表表达式的[] 改为() 即可变成生成器
2. 函数在返回值的时候出现yield 就变成生成器，而不是函数了

中括号换成小括号即可

```python
a = (i for i in range(3))

type(a)
# 下面是输出
generator
```

### 68. a = " hehheh "，去除首尾空格

```python
a = " hehheh "
a.strip()
# 下面是输出
"hehheh"
```

### 69. 举例sort 和sorted 对列表排序，list = [0, -1, 3, -10, 5, 9]

```python
# 列表排序
list = [0, -1, 3, -10, 5, 9]
list.sort(reverse=False)
print("list.sort 在list 基础上修改，无返回值", list)

list = [0, -1, 3, -10, 5, 9]
res = sorted(list, reverse=False)
print("sorted 有返回值是新的list", list)
print("返回值为", res)
# 下面是输出
# list.sort 在list 基础上修改，无返回值 [-10, -1, 0, 3, 5, 9]
# sorted 有返回值是新的list [0, -1, 3, -10, 5, 9]
# 返回值 [-10, -1, 0, 3, 5, 9]
```

### 70. 对list 排序foo = [-5, 8, 0, 4, 9, -4, -20, -2, 8,2, -4]，使用lambda 函数从小到大排序

```python
foo = [-5, 8, 0, 4, 9, -4, -20, -2, 8, 2, -4]
a = sorted(foo, key = lambda x: x)
# foo.sort()
print(a)
# 下面是输出
[-20, -5, -4, -4, -2, 0, 2, 4, 8, 8, 9]
```

### 71. 使用lambda 函数对list 排序foo = [-5, 8, 0, 4, 9, -4, -20, -2, 8, 2, -4]，输出结果为[0, 2, 4, 8, 8, 9, -2, -4, -4, -5, -20]，正数从小到大，负数从大到小

（传两个条件，x<0 和abs(x)），key 参数的值为一个函数，此函数只有一个参数且返回一个值用来进行比较。这个参数的调用是快速的因为key  指定的函数将准确的对每个元素调用；这里key 返回一个元组，并使用元组比较大小的逻辑来代替原元素判断大小。

```python
foo = [-5, 8, 0, 4, 9, -4, -20, -2, 8, 2, -4]
a = sorted(foo, key=lambda x: (x<0, abs(x)))
# foo.sort()
print(a)
# 下面是输出
[0, 2, 4, 8, 8, 9, -2, -4, -4, -5, -20]
```

### 72. 列表嵌套字典的排序，分别根据年龄和姓名排序

```python
foo = [{"name": "zs", "age": 19}, {"name": "ll", "age":54}, {"name": "wa", "age": 17}, {"name": "df", "age": 23}]
# 年龄从大到小
a = sorted(foo, key=lambda x: x["age"], reverse=True)、
print(a)
# 姓名从小到大
a = sorted(foo, key=lambda x: x["name"])
print(a)
# 下面是输出
[{"name": "ll", "age": 54}, {"name": "df", "age": 23}, {"name": "zs", "age": 19}, {"name": "wa", "age": 17}]
[{"name": "df", "age": 23}, {"name": "ll", "age":54}, {"name": "wa", "age": 17}, {"name": "zs", "age": 19}]
```

### 73. 列表嵌套元组，分别按照字母和数字排序

```python
foo = [("zs", 19), ("ll", 54), ("wa", 17), ("df", 23)]
a = sorted(foo, key=lambda x: x[1], reverse=True)
print(a)
a = sorted(foo, key=lambda x: x[0])
print(a)
# 下面是输出
[("ll", 54), ("df", 23), ("zs", 19), ("wa", 17)]
[("df", 23), ("ll", 54), ("wa", 17), ("zs", 19)]
```

### 74. 列表嵌套列表排序，年龄数字相同怎么办

添加参数，按照字母排序

```python
foo = [["ll", 54], ["df", 23], ["zs", 19], ["wa", 23], ["xf", 23]]
a = sorted(foo, key=lambda x: (x[1], x[0]))
print(a)
a = sorted(foo, key=lambda x: x[0])
print(a)
# 下面是输出
[["zs", 19], ["df", 23], ["wa", 23], ["xf", 23], ["ll", 54]]
[["df", 23], ["ll", 54], ["wa", 23], ["xf", 23], ["zs", 19]]
```

### 75. 根据键对字典排序

方法一：zip 函数

zip 函数用于将可迭代对象作为参数，将对象中对应的元素打包成一个个元组，然后返回这些元组组成的zip object（对象）

```python
dic = {"name": "zs", "sex": "man", "city": "bj"}
foo = zip(dic.keys(), dic.values())
# 转换成List列表
foo = [i for i in foo]
print("字典转成列表嵌套元组", foo)
# 字典嵌套元组排序
b = sorted(foo, key=lambda x: x[0])
print("根据键排序", b)
# 排序完成后构造新字典
new_dic = {i[0]:i[1] for i in b}
print("字典推导式构造新字典", new_dic)
# 下面是输出
字典转成列表嵌套元组[('name', 'zs'), ('sex', 'man'), ('city', 'bj')]
根据键排序[('city', 'bj'), ('name', 'zs'), ('sex', 'man')]
字典推导式构造新字典{'city': 'bj', 'name': 'zs', 'sex': 'man'}
```

方法二：不用zip

```python
dic = {"name": "zs", "sex": "man", "city": "bj"}
print("字典转成列表嵌套元组", foo)
print(dic.items())
b = sorted(dic.items(), key=lambda x: x[0])
print("根据键排序", b)
new_dic = {i[0]:i[1] for i in b}
print("字典推导式构造新字典", new_dic)
# 下面是输出
字典转成列表嵌套元组[('name', 'zs'), ('sex', 'man'), ('city', 'bj')]
根据键排序[('city', 'bj'), ('name', 'zs'), ('sex', 'man')]
字典推导式构造新字典{'city': 'bj', 'name': 'zs', 'sex': 'man'}
```

### 76. 列表推导式，字典推导式，生成器

```python
import random
td_list = [i for i in range(10)]
print("列表推导式", td_list, type(td_list))

ge_list = (i for i in range(10))
print("生成器", ge_list)

dic = {k:random.randint(4,9), for k in ["a", "b", "c", "d"]}
print("字典推导式", dic, type(dic))
# 下面是输出
列表推导式 [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]<class 'list'>
生成器<generator object <genexpr> at 0x00000018F61A0AF68>
字典推导式{'a': 7, 'b': 6, 'c': 9, 'd': 8} <class 'dict'>
```

### 77. s = "info:xiaoZheng 33 shandong", 用正则切分字符串输出['info', 'xiaoZhang', '33', 'shandong']

```python
s = "info:xiaoZheng 33 shandong"
res = re.split(r":| ", s)
print(res)
# 下面是输出
['info', 'xiaoZhang', '33', 'shandong']
```

### 78. 正则匹配以163.com 结尾的邮箱

```python
email_list = ["xiaoWang@163.com", "xiaoWang@163.comheihei", ".com.xiaoWang@qq.com"]
for email in email_list:
    ret = re.match("[\w]{4, 20}@163.com$", email)
    if ret:
        print("%s 是符合规定的邮件地址，匹配后的结果是：%s" %(email, ret.group()))
    else:
        print("%s 不符合要求" % email)
```

### 79. 递归求和

```python
# 递归完成1+2+3+...+10 的和
def geet_sum(num):
    if num >= 1:
        res = num + get_sum(num-1)
    else:
        res = 0
    return res

res = get_sum(10)
print(res)
# 下面是输出
55
```

### 80. 统计字符串中某字符出现次数

```python
str = "张三 美国 张三 哈哈 张 三"
res = str.count("张三")
print(res)
# 下面是输出
2
```

### 81. 字符串转化大小写

```python
str = "HHHuuu"
print(str.upper())
print(str.lower())
# 下面是输出
HHHUUU
hhhuuu
```

### 82. 用两种方法去空格

```python
str = "hello world ha ha"
res = str.replace(" ", "")
print(res)

list = str.split(" ")
res = "".join(list)
print(res)
# 下面是输出
helloworldhaha
helloworldhaha
```

### 83. 正则匹配不是以4和7结尾的手机号

```python
tels = ["13100001234", "18912344321", "10086", "18800007777"]
for tel in tels:
    ret = re.match("1\d{9}[0-3, 5-6, 8-9]", tel)
    if ret:
        print("想要的结果", ret.group())
    else:
        print("%s 不是想要的手机号" % tel)
# 下面是输出
13100001234 不是想要的手机号
想要的结果 18912344321
10086 不是想要的手机号
18800007777 不是想要的手机号
```

### 84. 简述Python  引用计数机制

Python 垃圾回收主要以引用计数为主，标记-清除和分代清除为辅的机制，其中标记-清除和分代回收主要是为了处理循环引用的难题

**引用计数算法**

当有1个变量保存了对象的引用时，此对象的引用计数就会加1

当使用del 删除变量指向的对象时，如果对象的引用计数不为1，比如3，那么此时只会让这个引用计数减1，即变为2，当再次调用del 时，变为1，如果再调用1次del，此时真的会把对象进行删除

### 85. int("1.4"), int(1.4) 输出结果

int("1.4") 报错，int(1.4) 输出1

### 86. 简述悲观锁和乐观锁

悲观锁，就是很悲观，每次去拿数据时都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block 直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。

乐观锁，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断下在此期间别人有没有去更新这个数据，可以使用版本号等机制，乐观锁适用于多读的应用类型，这样可以提高吞吐量。

### 87. Linux 命令重定向> 和>>

Linux 允许将命令执行结果重定向到一个文件，将本应该显示在终端上的内容输出/ 追加到指定文件中。

\> 表示输出，会覆盖文件中原有的内容

\>> 表示追加，会将内容追加到已有文件的末尾

### 88. Python 传参数是传值还是传址

Python 中函数参数是引用传递。对于不可变类型（数值型，字符串，元组），因为变量不能修改，所以运算不会影响到变量自身；而对于可变类型（列表，字典）来说，函数运算可能会更改传入的参数变量。

### 89. 求两个列表的交集，差集，并集

```python
a = [1,2,3,4]
b = [4,3,5,6]
# 在a 中的i, 并且也在b 中，就是交集
jj1 = [i for i in a if i in b]
jj2 = list(set(a).intersection(set(b)))

# b 中有而a 中没有的（非常高效）
cj1 = list(set(b).difference(set(a)))
# a 中有而b 中没有的（非常高效）
cj2 = list(set(a).difference(set(b)))

# 用union 方法
bj1 = list(set(a).union(set(b)))
print("交集", jj1)
print("交集", jj2)

print("并集", bj1)

print("差集", cj1)
print("差集", cj2)
# 下面是输出
交集[3,4]
交集[3,4]
并集[1,2,3,4,5,6]
差集[5,6]
差集[1,2]
```

### 90. 生成0-100 的随机数

random.random() 生成0-1 之间的随机小数，所以乘以100

```python
import random
res1 = 100*random.random()
# 随机整数
res2 = random.choice(range(0, 101))
res3 = random.randint(1, 100)
print(res1)
print(res2)
print(res3)
# 下面是输出
20.575902314280647
44
64
```

















