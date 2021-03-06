7. 主任务：根据字符串，打印对应回文字符。
 
7.1 任务简介与分析  
任务是由用户输入一个字符串，比如'赏花归去马如飞'，程序会打出：  

```python
.
            
             赏
            赏赏  
           赏花赏  
          赏花花赏  
         赏花归花赏  
        赏花归归花赏  
       赏花归去归花赏  
      赏花归去去归花赏  
     赏花归去马去归花赏  
    赏花归去马马去归花赏  
   赏花归去马如马去归花赏  
  赏花归去马如如马去归花赏  
 赏花归去马如飞如马去归花赏  
赏花归去马如飞飞如马去归花赏
```

第一行的字数为1，最后一行的字数为字符串内字数的2倍，也是图形的行数。毫无疑问我们要从给定的字符串中依次取出字来，然后考虑如何设计组成每一行。  
可以将这个图形分成奇数行和偶数行分别分析，有如下规律：

 - 对第i(i>1)行(奇数行)，是字符串第(i//2-1)个字及之前的字符 + 字符串第(i//2)字 + 字符串第(i//2-1)个字及之前的字符的逆序。
 - 对第j行(偶数行)，是字符串第(j//2)个字及之前的字符 + 字符串第(j//2)个字及之前的字符的逆序。

7.2 序列之字符串(str)

从第一讲我们就接触到了字符串，无论是在`print()`函数中使用还是作为单行、多行注释来使用，以往均将字符串作为一个整体。事实上，字符串有一串字符组成，每个字符都有一个索引，索引从0开始，字符串是序列的一种，很多使用方法与列表(list)类似，字符串类型的关键字是`str`。

- 键入如下代码并观察执行结果。

```python
# 示例代码 1

line = str()
print('用str()初始化：', line == '')

line = 'abcdefghijklmn'
print('a' in line)
print('a' not in line)

for ch in line:
    print(ch)
    
print('--------------分隔线----------------')
for i in range(10):
    print(i, line[i])
```

- 可以利用循环求得字符串的长度(含有的字符数)

```python
# 示例代码 2

def my_len(line):
    len = 0
    for ch in line:
        len += 1
    return len

# 主程序，测试函数用
my_len('abcde')
```

实际上，由于求序列长度(内含对象个数)的需求十分频繁，python提供的内置方法`len()`不但可得到`list`对象所含元素个数，其实是可以得到**序列**对象所含元素个数，当然也包括`str`对象。

- 键入如下代码并观察执行结果。

```python
# 示例代码 3

words = ['1','2','3','1','2','3']
print(len(words))

line = 'abcdefg'
print(len(line))
```

回到本节任务中来，先考虑一种极端情况，如果给定字符串内所有的字都相同，这任务就好做了。

- 键入如下代码并观察执行结果。

```python
# 示例代码 4

line = '天天天天天天'
for i in range(1, len(line)*2+1):
    for j in range(1,i):
        print(line[0], end='')
    print()
```

已经有点儿接近我们想要的了，接下来需要构造一定数量规律的空格放到每行前面，需要数数计算一下。
- 键入如下代码并观察执行结果。

```python
# 示例代码 5

line = '天天天天天天'
for i in range(1, len(line)*2+1):
    for k in range(len(line)*2-i,0,-1):
        print(' ', end='')
    for j in range(1,i):
        print(line[0], end='')
    print()
```

好，对于极端的字符串内所有字都相同的情况，已经可以处理，但是稍显繁琐，python提供了更简单的方式。
- 键入如下代码并观察执行结果。

```python
# 示例代码 6

line = '天'*6
for i in range(1, len(line)*2+1):
    print((len(line)*2-i)*' ' + line[0]*i, end='')
    print()
```

原来字符串可以相加还可以相乘。是的，python中的**序列**对象有加法运算和乘法运算，能够生成新的序列。

- 键入如下代码并观察执行结果。

```python
# 示例代码 7

line = 'Beijing language and culture university '
print('乘2：', line*2)
print('相加：''School of Information Studies of ' + line)
new_line = line * 3
print('乘3：', new_line)

print('-'*15 + '分隔线' + '-' *15)

numbers = [1,2,3]
print('乘3：', numbers * 3)
print('相加：', numbers + [4,5,6])
new_numbers = numbers * 4
print('乘4：', new_numbers)
```

回到任务，让我们先回顾下图形字符规律：

 - 对第i(i>1)行(奇数行)，是字符串第(i//2-1)个字及之前的字符 + 字符串第(i//2)字 + 字符串第(i//2-1)个字及之前的字符的逆序。
 - 对第j行(偶数行)，是字符串第(j//2)个字及之前的字符 + 字符串第(j//2)个字及之前的字符的逆序。  
 
其中有一部分内容是要得到原来字符串的整体中得到索引从0到x的新字符串，这个新字符串称为原字符串的**子串**。我们还要将该子串逆序。 

- 键入如下代码并观察执行结果。

```python
# 示例代码 8

line = '北京语言大学信息科学学院'
x = 4
sub_line = ''
for i in range(x):
    sub_line += line[i]
print(sub_line, end='')

for i in range(x-1, -1, -1):
    print(sub_line[i], end='')
```

非常好，看到结果难免会有些小激动。但是在已知索引截取子串及逆序时，for循环需要细心考虑`range()`的边界，且稍显笨拙。幸运的是python提供了更优雅的办法。
- 键入如下代码并观察执行结果。

```python
# 示例代码 9
line = '北京语言大学信息科学学院'
x = 4
print(line[0:x] + line[x-1:0:-1] + line[0])
```

本段代码中形如`序列[m:n:i]`的操作称为**序列切片**，就是从序列中取出索引在[m,n)之间，以间隔为i的所有对象，默认i为1。

- 键入如下代码并观察执行结果。

```python
# 示例代码 10

line = '北京语言大学信息科学学院'

print(line[2:6])
print(line[2:8:2])
print(line[8:1:-2])

print('-'*15 + '分割线' + '-'*15)

print(line[0:6])
print(line[:6])
print(line[6:12])
print(line[6:])
print(line[:])
print(line[::2])

print('-'*15 + '分割线' + '-'*15)

print(line[-5:-2])
print(line[0:4] + line[3::-1])   # 请对比示例代码 9
```

如果切片起始或者结束位置为字符串边界，则可以省略。此外，还可以从右向左索引，最右侧边界字符的索引位置为`-1`，向左依次减小。  
好，我们再次回到任务，来完成我们的回文图形。还是参考之前总结的规律：

 - 对第i(i>1)行(奇数行)，是字符串第(i//2-1)个字及之前的字符 + 字符串第(i//2)字 + 字符串第(i//2-1)个字及之前的字符的逆序。
 - 对第j行(偶数行)，是字符串第(j//2)个字及之前的字符 + 字符串第(j//2)个字及之前的字符的逆序。

- 键入如下代码并观察执行结果。

```python
# 示例代码 11

line = '赏花归去马如飞'

for i in range(1, len(line)*2):
    if i == 1:
        print(line[0])
    elif i%2 == 1:
        print(line[:i//2] + line[i//2] + line[i//2-1::-1])
    else:
        print(line[:i//2] + line[i//2-1::-1])
```

已经基本达到目的，只是图案是倾斜的，如何将这个‘侧妃扶正’，暂留待读者思考解决，此外，可将这段代码做成函数，可以实现任意字符串的回文图案生成。  
事实上，上述代码中如果将`line`类型初始化为其他序列类型，也能够完成同样的功能。

7.4 序列之元组(tuple)  

- 键入如下代码并观察执行结果。

```python
# 示例代码 12

line = ('赏', '花', '归', '去', '马', '如', '飞')

for i in range(1, len(line)*2):
    if i == 1:
        print((line[0],))
    elif i%2 == 1:
        print(line[:i//2] + (line[i//2],) + line[i//2-1::-1])
    else:
        print(line[:i//2] + line[i//2-1::-1])
```
python内置类型中，与列表(list)及字符串(str)最相近的序列是元组(tuple)，`tuple`即为元组的关键字。  

- 键入如下代码并观察执行结果。

```python
# 示例代码 13

numbers = ()
print(numbers)

numbers = tuple()
print(numbers)

numbers = (1)
print(numbers)

numbers = (1,)
print(numbers)

numbers = 1,
print(numbers)

numbers = (1, 2, 3, 4, 5)
print(numbers)

numbers = 1, 2, 3, 4, 5
print(numbers)

```

可见元组(tuple)的初始化与列表(list)类似，元组用`()`，`()`也可以省略，但建议只在不降低程序可读性时省略。

- 键入如下代码并观察执行结果。

```python
# 示例代码 14

numbers = [10, 20, 30]
x, y = numbers
print(x)
print(y)

a, b, c = x
print(a, b, c)

c, a, b = a, b, c
print(a, b, c)
```

序列内的对象，可以按照对象个数同时赋值给多个变量/对象，称为**解耦**。
本段中语句`c, a, b= a, b, c`，右侧实际上是利用三个变量形成了一个元组，然后解耦，在一行中同时完成了3个变量之间值的交换。

- 键入如下代码并观察执行结果。

```python
# 示例代码 15

numbers = 1, 2, 3, 4, 5
for number in numbers:
    print(number)

print('-'*30)

numbers *= 2
for i in range(10):
    print(numbers[i])

print('-'*30)
numbers = (1, 2, 3, 4, 5) * 4
print(numbers)
```

可见元组的遍历、索引及乘法与列表及字符串类似。

- 键入如下代码并观察执行结果。

```python
# 示例代码 16

def three_return_value():
    return 1, 2, 3
    
print(three_return_value())

def three_return_word():
    return 'one', 'two', 'three'

print(three_return_word())

def two_return_tuple():
    numbers1 = 1,2,3
    numbers2 = 4,5,6
    return numbers1, numbers2

print(two_return_tuple())

def two_return_list():
    numbers1 = [1,2,3]
    numbers2 = [4,5,6]
    return numbers1, numbers2

print(two_return_list())
```

可见，如函数返回多个值，则多个值将以元组的形式返回。同时，元组与列表内的元素还可以是元组或列表，一般称为**嵌套**。

- 键入如下代码并观察执行结果。

```python
# 示例代码 17

numbers = [(1,2,3), (4,5,6), (7,8,9)]
print(numbers[0][0])
print(numbers[1][0])
print(numbers[2][2])
print('-'*30)
for number in numbers:
    print(number)
    for num in number:
        print(num)
```

虽然看似复杂，但仍然可以利用与先前类似的方法遍历以及利用索引取对象，索引顺序是从外层到内层。  
回到用非字符串解决回文图案的问题，如何利用元组或列表实现以及如何将图案扶正，留待读者自行思考解决。

7.5 可变与不可变数据类型    

有些读者也许早已经憋坏了，字符串还算有特点，那个元组和列表有区别吗？除了括号不同，弄两个名词，弄两种类型有必要吗？  
另外，列表中有一个`append()`方法，但是在介绍字符串及元组时，怎么一直没有介绍或者使用呢？

- 键入如下代码并观察执行结果。

```python
# 示例代码 18

numbers = []
numbers.append(10)
print(numbers)

nums = ()
nums.append(10)     # 不能添加
```

提示错误，`tuple`对象没有这个方法。

- 键入如下代码并观察执行结果。

```python
# 示例代码 19

numbers = [10,20]
numbers.append(30)
print(numbers)

line = 'abcd'
line.append('e')     #错误，不能添加
```

提示错误，`str`对象没有这个方法。  
其实，不但不能利用`append()`函数添加对象，也不能通过任何方式改变元组或字符串对象的值。

- 键入如下代码并观察执行结果。

```python
# 示例代码 20

numbers = [1,2,3,4]
numbers[2] = 8
print(numbers)

nums = 1,2,3,4
nums[2] = 8         #错误，不支持
```
```python
line = 'abcde'
line[2] = 'f'         #错误，不支持
```

提示错误,元组及字符串对象不支持赋值。  
我们再来看看序列切片操作时赋值。
- 键入如下代码并观察执行结果。

```python
# 示例代码 21

numbers = [1, 2, 3, 4, 5]
numbers[2:4] = 10,11,12
print(numbers)

nums = 1,2,3,4,5
nums[2:4] = 10,11,12        #错误，不支持

line = 'abcde'
line[2:4] = 'ghi'           # 错误，不支持
```
依旧是提示错误,元组及字符串对象不支持赋值。  

- 键入如下代码并观察执行结果。

```python
# 示例代码 22

numbers = [1,2,3,4]
numbers.remove(2)
print(numbers)

nums = 1,2,3,4
nums.remove(2)      #错误，不能删除
```
```python
line = 'abcde'
line.remove('b')      #错误，不能删除
```

提示错误，元组及字符串对象没有这个方法。  
本段代码中的`remove()`是列表的一个方法，可以删除列表中的指定对象，用法为：`列表.remove(对象)`。  

综上可见，元组与字符串一旦被初始化，其值就不能被改变，python语言称为**不可变数据类型**，而类似列表这样的数据类型，初始化后，其值可以改变，就称为**可变数据类型**。  
除了元组及字符串外，python中数值常量也是不可变数据类型，比如说，`5`就是`5`，不可能等于`6`，也不能等于`5.1`。  
好，数值常量的比较容易理解。那么可变与不可变的区分，比如列表与元组区别开有什么意义呢？元组不可变，因此对元组操作很多时候还还需要建立新元组，多麻烦。全部设计成可变的不是更好吗？实际上，元组单独作为一种数据类型，有其特别的考虑：一个原因是相同操作时，一般元组比列表速度更快；另外，利用元组的不可变性可以保护某些本来就不打算更改的序列；最后，后续一些数据类型如`dict`，只能用不可变类型作为键。

- 键入如下代码并观察执行结果。

```python
# 示例代码 23

numbers_1 = [1,2,3,4,5]
numbers_2 = numbers_1
print(numbers_2)

numbers_1[0] = 9
print(numbers_2)
```

哎呦，好奇怪，明明没有对`numbers_2`进行操作，只改变了`numbers_1`啊，怎么会连带`numbers_2`的值也给改了？这不是我的本意。  
让我们从赋值开始，详细解释下原因。  
什么是赋值？python语言中，赋值就是让一个变量/对象指向一个具体值，即常量，如：`number = 10`，就是先产生一个常量`10`，然后让`number`指向`10`。当使用变量/对象值运算时候，就取出其指向的常量具体值来参与运算。  
常量是不可变的，因此如果将`number`再次赋值，如`number = 20`，则是先产生一个常量`20`，然后让`number`指向`20`。为变量/对象再次赋值就是更改变量/对象的指向。  
一个变量/对象，在一个时刻，只能指向一个常量。此时，`number`仅指向`20`，不再指向`10`。  
一个常量，可以被多个变量/对象指向。  

（此处画图说明）
（此处画图说明）
（此处画图说明）

- 键入如下代码并观察执行结果。

```python
# 示例代码 24

number_1 = 10
number_2 = number_1

print(id(number_1), id(number_2))

print('-'*30)

number_1 = 20
print(id(number_1), id(number_2))

```

本段代码中的`id()`是一个函数，可以返回一个变量/对象在计算机中的`唯一标识`，用法是`id(对象)`，这个唯一标识其实就是对象在计算机(内存)中存放的地址。  
可以看到，当将`number_1`赋值给`number_2`后，两者`id`相同，此时`number_1`与`number_2`都指向常量`10`，类似同一个对象起了两个名称，可以说是互为**别名**。  
当`number_1`被再次赋值后，其`id`发生了改变，与`number_2`没有关系了。  

- 将先前‘非我们本意’的代码，加入`id()`函数仔细观察：

```python
# 示例代码 25

numbers_1 = [1,2,3,4,5]
numbers_2 = numbers_1

print('n1={}, n2={}'.format(id(numbers_1), id(numbers_2)))

numbers_1[0] = 9
print(numbers_1, numbers_2)
print('n1={}, n2={}'.format(id(numbers_1), id(numbers_2)))
```

由本段代码可知，`numbers_1`与`numbers_2`的`id`相同，是指向同一个列表`[1,2,3,4,5]`的两个‘别名’，列表是可变的，因此这个列表变成`[0,2,3,4,5]`之后，指向这个列表的两个别名，值自然都变成了`[0,2,3,4,5]`。  
现在问题来了，如何能得到两个真正不同的列表，其值是相同但并非指向同一个对象？

```python
# 示例代码 26

numbers_1 = [1,2,3,4,5]
numbers_2 = [1,2,3,4,5]
print(id(numbers_1), id(numbers_2))
```

上面这样是可以的，但是一个是比较重复，二来在多数程序中，可能列表的值是计算得到，是变量，难以这样显示的直接赋值。我们不考虑`numbers_1 = numbers_2`，因为已经确定行不通。

```python
# 示例代码 27

numbers_1 = [1,2,3,4,5]
numbers_2 = numbers_1.copy()
print(id(numbers_1), id(numbers_2))

numbers_1[0] = 9
print(numbers_1, numbers_2)
```

python内置浅拷贝方法`copy()`，可以对可变序列进行浅拷贝复制，用法是：`可变序列.copy`。称为浅拷贝(shallow copy)的原因，是因为拷贝的确实不够深入。在实际使用中，浅拷贝多用切片来代替（也是浅拷贝）。最后，如果不注意，还是有产生‘非我们本意’情况的可能。

```python
# 示例代码 28

numbers_1 = [1,2,3,4,[9,8,7]]
numbers_2 = numbers_1.copy()
numbers_3 = numbers_1[:]
print(id(numbers_1), id(numbers_2), id(numbers_3))

numbers_1[4][0] = 5
print(numbers_1, numbers_2, numbers_3)
```

我们非常遗憾的看到`numbers_1`、`numbers_2`及`numbers_3`还是同时变化了。  
问题出在浅拷贝上，浅拷贝在拷贝这种嵌套结构(也称为复合结构)时，对复合结构内部的结构如序列，并非将其值拷贝，而是仅建立了指向。此时被拷贝对象和新对象虽然指向不同的复合结构，但是这两个复合结构内部，都存在指向相同结构的指向。（用图说明）。  
上例中，浅拷贝后，`numbers_1`、`numbers_2`及`numbers_3`的索引为4的位置，同时指向列表`[9,8,7]`，将`[9,8,7]`改变成`[5,8,7]`以后，自然同时影响`numbers_1`、`numbers_2`及`numbers_3`。  
与浅拷贝对应，就有深拷贝(deepcopy)。

```python
# 示例代码 29

import copy

numbers_1 = [1,2,3,4,[9,8,7]]
numbers_2 = copy.deepcopy(numbers_1)
print(id(numbers_1), id(numbers_2))

numbers_1[4][0] = 5
print(numbers_1, numbers_2)
```

要使用`deepcopy()`方法，需要先导入`copy`模块，基本用法为：`copy.deepcopy(对象)`，实现深拷贝。


7.6 拓展与总结  

- 序列通用操作
用下表来进行说明，令`x = 1`，`i=2`，`n=3`，`j=7`，`k=2`，`s = [1,2,3,4,5,6,7]`，`t = [8,9,10]`。

操作|结果或`s`的值|说明
---|---|---
`x in s`|`True`|如s中有一个对象等于x，返回`True`否则返回`False`
`x not in s`|`True`|如s中有一个对象等于x，返回`False`否则返回`True`
`s+t`|`[1,2,3,4,5,6,7,8,9,10]`|将s与t连接
`n*t`或`t*n`|`[8,9,10,8,9,10,8,9,10]`|相当于将`t`自加`n`次
`s[i]`|`3`|序列`s`的第`i`个对象，起始对象为第`0`个
`s[i:j]`|`[3,4,5,6,7]`|`s`从`i`到`j`之间的切片(不包含`j`)
`s[i:j:k]`|`[3,5,7]`|`s`从`i`到`j`之间间隔为`k`的切片(不包含`j`)
`len(s)`|`7`|`s`的长度(对象个数)
`max(s)`|`1`|`s`中最小的对象
`min(s)`|`7`|`s`中最大的对象
`s.index(x)`|`1`|`s`中`x`第一次出现时的索引位置
`s.count(x)`|`1`|`s`中`x`出现的个数

- 可变序列操作
用下表来进行说明，令`x = 1`，`i=2`，`n=3`，`j=7`，`k=2`，`s = [1,2,3,4,5,6,7,8]`，`t = [9,10,11]`。

操作|结果或`s`的值|说明
---|---|---
`s[i] = x`|`[1,2,1,4,5,6,7,8]`|`s`中第`i`个对象替换成`x`
`s[i:j] = t`|`[1,2,9,10,11,8]`|将`s`中`i`到`j`(不包括`j`位置对象)的切片替换为序列`t`中的对象。
`del s[i:j]`|`[1,2,8]`|等价于`s[i:j] = []`
`s[i:j:k] = t`|`[1,2,9,4,10,6,11,8]`|将`s`中`i`到`j`(不包括`j`位置对象)间隔为`k`的切片依次替换为序列`t`中的对象。
`del s[i:j:k]`|`[1,2,4,6,8]`|将`s`中`i`到`j`(不包括`j`位置对象)间隔为`k`位置的对象删除。
`s.append(x)`|`[1,2,3,4,5,6,7,8,1]`|将`x`加入到`s`的末端，等价于`s[len(s):len(s)] = [x]`
`s.clear()`|`[]`|清空`s`中所有元素，等价于`del s[:]`，或`s[:] = []`
`s.copy()`|`[1,2,3,4,5,6,7,8]`|`s`的浅拷贝，等价于s[:]
`s += t` 或者 `s.extend(t)`|`[1,2,3,4,5,6,7,8,9,10,11]`|`s`后面扩展`t`
`t *= n`|`[9,10,11,9,10,11,9,10,11]`|`t`中内容重复n次后再复制给`t`
`s.insert(i, x)`|`[1,2,1,3,4,5,6,7,8]`|在`s`的第`i`个位置插入元素`x`，等价于`s[i:i] = [x]`。
`s.pop([i])`|`[1,2,4,5,6,7,8]` 并返回`3`|从`s`中删除第`i`个元素，并将其作为返回值
`s.remove(x)`|`[2,3,4,5,6,7,8]`|从`s`中删除第一个`x`对象。
`s.reverse()`|`[8,7,6,5,4,3,2,1]`|将`s`进行就地反转。

- 切片索引图

```python
 -+---+---+---+---+---+---+
 | P | y | t | h | o | n |
 +---+---+---+---+---+---+
 0   1   2   3   4   5   6
-6  -5  -4  -3  -2  -1
```

7.7 完整代码  

```python
#coding: utf-8
# 示例代码 30

def plalindrome(line):
    for i in range(1, len(line)*2):
        if i == 1:
            print(' '*(len(line)*2-1) +line[0])
        elif i%2 == 1:
            print(' '*(len(line)*2-i) + line[:i//2] + line[i//2] + line[i//2-1::-1])
        else:
            print(' '*(len(line)*2-i) + line[:i//2] + line[i//2-1::-1])

def main():
    text = '赏花归去马如飞'
    plalindrome(text)
    

if __name__ == '__main__':
    main()
```

7.9 习题

练习一：实现`reverse(s)`函数，功能与`s.reverse()`相同。

练习二：写函数，根据给定符号和行数，打印相应直角三角形，等腰三角形及其他形式的三角形。

练习三：将任务4中的英语动词变复数的函数，完整实现。

练习三、四：写函数，根据给定符号，上底、下底、高，打印各种梯形。

练习五：写函数，根据给定符号，打印各种菱形。

练习六：文本加密解密。  
加密：读入一个txt文件(全英文，假设单词词长最长为10)，得到每个单词的长度`n`，随机生成一个9位的数字，将`n`与这个9位的数字连接，形成一个10位的数字，作为密匙`key`。
依照`key`中各个数字对单词中每一个对应位置的字母进行向后移位，对长度不到10的单词，移位后利用随机字母补全到10，最终形成以10个字母为一个单词，并以空格分割的加密文本，存入文件。  
解密：给定该文本文件并给定key(10位数字)，恢复原来的文本。  
(提示，利用`ord()`及`chr()`函数，`ord(x)`是取得字符`x`的ASCII码，`chr(n)`是取得整数n(代表ASCII码)对应的字符。例：`ord(a)`的值为`97`，`chr(97)`的值为`'a'`，因字母`a`的ASCII码值为`97`。)

练习七：与本小节任务基本相同，但要求打印回文字符倒三角形。
