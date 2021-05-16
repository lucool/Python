# Python 炫技操作：推导式的五种写法

推导式（英文名：comprehensions），也叫解析式，是Python的一种独有特性。

推导式是可以从一个数据序列构建另一个新的数据序列的结构体。

总共有四种推导式：

1. 列表(list)推导式
2. 字典(dict)推导式
3. 集合(set)推导式
4. 生成器推导式

**1.列表推导式**

列表推导式的基本格式

```python
new_list = [expression for_loop_expression if condition]
```

举个例子。

我想找出一个数值列表中为偶数的元素，并组成新列表，通常不用列表推导式，可以这么写

```python
old_list = [0,1,2,3,4,5]
new_list = []
for item in old_list:    
    if item % 2 == 0:
        new_list.append(item)
print(new_list) # output: [0, 2, 4]
```

一个简单的功能，写的代码倒是不少。

如果使用了列表推导式，那就简洁多了，而且代码还变得更加易读了。

```
old_list = [0,1,2,3,4,5]
new_list = [item for item in old_list if item % 2 == 0]
print(new_list) # output: [0, 2, 4]
```

**2.字典推导式**

字典推导式的基本格式，和 列表推导式相似，只是把 `[]` 改成了 `{}`，并且组成元素有两个：key 和 value，要用 `key_expr: value_expr` 表示。

```python
new_dict ={ key_expr: value_expr for_loop_expression if condition }
```

举个例子。

我想从一个包含所有学生成绩信息的字典中，找出数学考满分的同学。

```python
old_student_score_info = {
    "Jack": {
        "chinese": 87,
        "math": 92,
        "english": 78
    },
    "Tom": {
        "chinese": 92,
        "math": 100,
        "english": 89
    }
}

new_student_score_info = {name: scores for name, scores in old_student_score_info.items() if scores["math"] == 100}
print(new_student_score_info)
# output: {'Tom': {'chinese': 92, 'math': 100, 'english': 89}}
```

**3.集合推导式**

集合推导式跟列表推导式也是类似的。唯一的区别在于它使用大括号`{}`，组成元素也只要一个。

基本格式

```
new_set = { expr for_loop_expression if condition }
```

举个例子

我想把一个数值列表里的数进行去重处理

```
old_list = [0,0,0,1,2,3]
new_set = {item for item in old_list}
print(new_set)# {0, 1, 2, 3}
```

**4.生成器推导式**

生成器推导式跟列表推导式，非常的像，只是把 `[]` 换成了 `()`

- 列表推导式：生成的是新的列表
- 生成器推导式：生成的是一个生成器

直接上案例了，找出一个数值列表中所有的偶数

```
old_list = [0,1,2,3,4,5]
new_list = (item for item in old_list if item % 2 == 0)
new_list #<generator object <genexpr> at 0x10292df10>>>> next(new_list)0>>> next(new_list) #2
```

**5.嵌套推导式**

for 循环可以有两层，甚至更多层，同样的，上面所有的推导式，其实都可以写成嵌套的多层推导式。

但建议最多嵌套两层，最多的话，代码就会变得非常难以理解。

举个例子。

我想打印一个乘法表，使用两个for可以这样写

```
for i in range(1, 10):    
    for j in range(1, i+1):        
        print('{}x{}={}\t'.format(j, i, i*j), end='')    
    print("")
```

输出如下

```
1x1=1    
1x2=2    2x2=4   
1x3=3    2x3=6   3x3=9   
1x4=4    2x4=8   3x4=12  4x4=16  
1x5=5    2x5=10  3x5=15  4x5=20  5x5=25  
1x6=6    2x6=12  3x6=18  4x6=24  5x6=30  6x6=36  
1x7=7    2x7=14  3x7=21  4x7=28  5x7=35  6x7=42  7x7=49  
1x8=8    2x8=16  3x8=24  4x8=32  5x8=40  6x8=48  7x8=56  8x8=64  
1x9=9    2x9=18  3x9=27  4x9=36  5x9=45  6x9=54  7x9=63  8x9=72  9x9=81
```

如果使用嵌套的列表推导式，可以这么写

```
print('\n'.join([' '.join(['%2d *%2d = %2d' % (col, row, col * row) for col in range(1, row + 1)]) for row in range(1, 10)]))
```

# Python验证常见的50个正则表达式

什么是正则表达式？

正则表达式（Regular Expression）通常被用来检索、替换那些符合某个模式(规则)的文本。

此处的Regular即是规则、规律的意思，Regular Expression即“描述某种规则的表达式”之意。

本文收集了一些常见的正则表达式用法，方便大家查询取用，并在最后附了详细的正则表达式语法手册。

案例包括：**「邮箱、身份证号、手机号码、固定电话、域名、IP地址、日期、邮编、密码、中文字符、数字、字符串」**

**Python如何支持正则？**

我用的是python来实现正则，并使用Jupyter Notebook编写代码。

Python通过re模块支持正则表达式，re 模块使 Python 语言拥有全部的正则表达式功能。

这里要注意两个函数的使用：

`re.compile`用于编译正则表达式，生成一个正则表达式（ Pattern ）对象;

`.findall`用于在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。

```
# 导入re模块import re
```

**1.邮箱**

包含大小写字母，下划线，阿拉伯数字，点号，中划线

表达式：

```
[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(?:\.[a-zA-Z0-9_-]+)
```

案例：

```
pattern = re.compile(r"[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(?:\.[a-zA-Z0-9_-]+)")strs = '我的私人邮箱是zhuwjwh@outlook.com，公司邮箱是123456@qq.org，麻烦登记一下？'result = pattern.findall(strs)print(result)
['zhuwjwh@outlook.com', '123456@qq.org']
```

2. **身份证号**

xxxxxx yyyy MM dd 375 0   十八位

- 地区：[1-9]\d{5}
- 年的前两位：(18|19|([23]\d))    1800-2399
- 年的后两位：\d{2}
- 月份：((0[1-9])|(10|11|12))
- 天数：(([0-2][1-9])|10|20|30|31)      闰年不能禁止29+
- 三位顺序码：\d{3}
- 两位顺序码：\d{2}
- 校验码：[0-9Xx]

表达式：

```
[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]
```

案例：

```
pattern = re.compile(r"[1-9]\d{5}(?:18|19|(?:[23]\d))\d{2}(?:(?:0[1-9])|(?:10|11|12))(?:(?:[0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]")strs = '小明的身份证号码是342623198910235163，手机号是13987692110'result = pattern.findall(strs)print(result)
['342623198910235163']
```

3. **国内手机号码**

手机号都为11位，且以1开头，第二位一般为3、5、6、7、8、9 ，剩下八位任意数字
例如：13987692110、15610098778

表达式：

```
1(3|4|5|6|7|8|9)\d{9}
```

案例：

```
pattern = re.compile(r"1[356789]\d{9}")strs = '小明的手机号是13987692110，你明天打给他'result = pattern.findall(strs)print(result)
['13987692110']
```

4. **国内固定电话**

区号3~4位，号码7~8位

例如：0511-1234567、021-87654321

表达式：

```
\d{3}-\d{8}|\d{4}-\d{7}
```

案例：

```
pattern = re.compile(r"\d{3}-\d{8}|\d{4}-\d{7}")strs = '0511-1234567是小明家的电话，他的办公室电话是021-87654321'result = pattern.findall(strs)print(result)
['0511-1234567', '021-87654321']
```

5. **域名**

包含http:\\或https:\\

表达式：

```
(?:(?:http:\/\/)|(?:https:\/\/))?(?:[\w](?:[\w\-]{0,61}[\w])?\.)+[a-zA-Z]{2,6}(?:\/)
```

案例：

```
pattern = re.compile(r"(?:(?:http:\/\/)|(?:https:\/\/))?(?:[\w](?:[\w\-]{0,61}[\w])?\.)+[a-zA-Z]{2,6}(?:\/)")strs = 'Python官网的网址是https://www.python.org/'result = pattern.findall(strs)print(result)
['https://www.python.org/']
```

6. **IP地址**

IP地址的长度为32位(共有2^32个IP地址)，分为4段，每段8位，用十进制数字表示
每段数字范围为0～255，段与段之间用句点隔开　

表达式：

```
((?:(?:25[0-5]|2[0-4]\d|[01]?\d?\d)\.){3}(?:25[0-5]|2[0-4]\d|[01]?\d?\d))
```

案例：

```
pattern = re.compile(r"((?:(?:25[0-5]|2[0-4]\d|[01]?\d?\d)\.){3}(?:25[0-5]|2[0-4]\d|[01]?\d?\d))")strs = '''请输入合法IP地址，非法IP地址和其他字符将被过滤！增、删、改IP地址后，请保存、关闭记事本！192.168.8.84192.168.8.85192.168.8.860.0.0.1256.1.1.1192.256.256.256192.255.255.255aa.bb.cc.dd'''result = pattern.findall(strs)print(result)
['192.168.8.84', '192.168.8.85', '192.168.8.86', '0.0.0.1', '56.1.1.1', '192.255.255.255']
```

7. **日期**

常见日期格式：yyyyMMdd、yyyy-MM-dd、yyyy/MM/dd、yyyy.MM.dd

表达式：

```
\d{4}(?:-|\/|.)\d{1,2}(?:-|\/|.)\d{1,2}
```

案例：

```
pattern = re.compile(r"\d{4}(?:-|\/|.)\d{1,2}(?:-|\/|.)\d{1,2}")strs = '今天是2020/12/20，去年的今天是2019.12.20，明年的今天是2021-12-20'result = pattern.findall(strs)print(result)
['2020/12/20', '2019.12.20', '2021-12-20']
```

8. **国内邮政编码**

我国的邮政编码采用四级六位数编码结构
前两位数字表示省（直辖市、自治区）
第三位数字表示邮区；第四位数字表示县（市）
最后两位数字表示投递局（所）

表达式：

```
[1-9]\d{5}(?!\d)
```

案例：

```
pattern = re.compile(r"[1-9]\d{5}(?!\d)")strs = '上海静安区邮编是200040'result = pattern.findall(strs)print(result)
['200040']
```

9. **密码**

密码(以字母开头，长度在6~18之间，只能包含字母、数字和下划线)

表达式：

```
[a-zA-Z]\w{5,17}
```

强密码(以字母开头，必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间)

表达式：

```
[a-zA-Z](?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}
pattern = re.compile(r"[a-zA-Z]\w{5,17}")strs = '密码：q123456_abc'result = pattern.findall(strs)print(result)
['q123456_abc']
pattern = re.compile(r"[a-zA-Z](?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}")strs = '强密码：q123456ABc，弱密码：q123456abc'result = pattern.findall(strs)print(result)
['q123456ABc，']
```

10. **中文字符**

表达式：

```
[\u4e00-\u9fa5]
```

案例：

```
pattern = re.compile(r"[\u4e00-\u9fa5]")strs = 'apple：苹果'result = pattern.findall(strs)print(result)
['苹', '果']
```

11. **数字**

- 验证数字：`^[0-9]*$`
- 验证n位的数字：`^\d{n}$`
- 验证至少n位数字：`^\d{n,}$`
- 验证m-n位的数字：`^\d{m,n}$`
- 验证零和非零开头的数字：`^(0|[1-9][0-9]*)$`
- 验证有两位小数的正实数：`^[0-9]+(.[0-9]{2})?$`
- 验证有1-3位小数的正实数：`^[0-9]+(.[0-9]{1,3})?$`
- 验证非零的正整数：`^\+?[1-9][0-9]*$`
- 验证非零的负整数：`^\-[1-9][0-9]*$`
- 验证非负整数（正整数 + 0） `^\d+$`
- 验证非正整数（负整数 + 0） `^((-\d+)|(0+))$`
- 整数：`^-?\d+$`
- 非负浮点数（正浮点数 + 0）：`^\d+(\.\d+)?$`
- 正浮点数 `^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$`
- 非正浮点数（负浮点数 + 0） `^((-\d+(\.\d+)?)|(0+(\.0+)?))$`
- 负浮点数 `^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$`
- 浮点数 `^(-?\d+)(\.\d+)?$`

12. **字符串**

- 英文和数字：`^[A-Za-z0-9]+$ 或 ^[A-Za-z0-9]{4,40}$`
- 长度为3-20的所有字符：`^.{3,20}$`
- 由26个英文字母组成的字符串：`^[A-Za-z]+$`
- 由26个大写英文字母组成的字符串：`^[A-Z]+$`
- 由26个小写英文字母组成的字符串：`^[a-z]+$`
- 由数字和26个英文字母组成的字符串：`^[A-Za-z0-9]+$`
- 由数字、26个英文字母或者下划线组成的字符串：`^\w+$ 或 ^\w{3,20}$`
- 中文、英文、数字包括下划线：`^[\u4E00-\u9FA5A-Za-z0-9_]+$`
- 中文、英文、数字但不包括下划线等符号：`^[\u4E00-\u9FA5A-Za-z0-9]+$ 或 ^[\u4E00-\u9FA5A-Za-z0-9]{2,20}$`
- 可以输入含有^%&',;=?$\”等字符：`[^%&',;=?$\x22]+`
- 禁止输入含有~的字符：`[^~\x22]+`

附：正则表达式语法详解

| 字符           | 描述                                                         |
| :------------- | :----------------------------------------------------------- |
| `\`            | 将下一个字符标记为一个特殊字符（File Format Escape，清单见本表）、或一个原义字符（Identity Escape，有^$()*+?.[{\|共计12个)、或一个向后引用（backreferences）、或一个八进制转义符。例如，“`n`”匹配字符“`n`”。“`\n`”匹配一个换行符。序列“`\\`”匹配“`\`”而“`\(`”则匹配“`(`”。 |
| `^`            | 匹配输入字符串的开始位置                                     |
| `$`            | 匹配输入字符串的结束位置                                     |
| `*`            | 匹配前面的子表达式零次或多次。例如，zo*能匹配“`z`”、“`zo`”以及“`zoo`”。*等价于{0,}。 |
| `+`            | 匹配前面的子表达式一次或多次。例如，“`zo+`”能匹配“`zo`”以及“`zoo`”，但不能匹配“`z`”。+等价于{1,}。 |
| `?`            | 匹配前面的子表达式零次或一次。例如，“`do(es)?`”可以匹配“`does`”中的“`do`”和“`does`”。?等价于{0,1}。 |
| `{n}`          | n是一个非负整数。匹配确定的n次。例如，“`o{2}`”不能匹配“`Bob`”中的“`o`”，但是能匹配“`food`”中的两个o。 |
| `{n,}`         | n是一个非负整数。至少匹配n次。例如，“`o{2,}`”不能匹配“`Bob`”中的“`o`”，但能匹配“`foooood`”中的所有o。“`o{1,}`”等价于“`o+`”。“`o{0,}`”则等价于“`o*`”。 |
| `{n,m}`        | m和n均为非负整数，其中n<=m。最少匹配n次且最多匹配m次。例如，“`o{1,3}`”将匹配“`fooooood`”中的前三个o。“`o{0,1}`”等价于“`o?`”。请注意在逗号和两个数之间不能有空格。 |
| `?`            | 非贪心量化（Non-greedy quantifiers）：当该字符紧跟在任何一个其他重复修饰符（*,+,?，{n}，{n,}，{n,m}）后面时，匹配模式是**「非」**贪婪的。非贪婪模式尽可能少的匹配所搜索的字符串，而默认的贪婪模式则尽可能多的匹配所搜索的字符串。例如，对于字符串“`oooo`”，“`o+?`”将匹配单个“`o`”，而“`o+`”将匹配所有“`o`”。 |
| `.`            | 匹配除“`\r`”“`\n`”之外的任何单个字符。要匹配包括“`\r`”“`\n`”在内的任何字符，请使用像“`(.\|\r\|\n)`”的模式。 |
| `(pattern)`    | 匹配pattern并获取这一匹配的子字符串。该子字符串用于向后引用。所获取的匹配可以从产生的Matches集合得到，在VBScript中使用SubMatches集合，在JScript中则使用$0…$9属性。要匹配圆括号字符，请使用“`\(`”或“`\)`”。可带数量后缀。 |
| `(?:pattern)`  | 匹配pattern但不获取匹配的子字符串（shy groups），也就是说这是一个非获取匹配，不存储匹配的子字符串用于向后引用。这在使用或字符“`(\|)`”来组合一个模式的各个部分是很有用。例如“`industr(?:y\|ies)`”就是一个比“`industry\|industries`”更简略的表达式。 |
| `(?=pattern)`  | 正向肯定预查（look ahead positive assert），在任何匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如，“`Windows(?=95\|98\|NT\|2000)`”能匹配“`Windows2000`”中的“`Windows`”，但不能匹配“`Windows3.1`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。 |
| `(?!pattern)`  | 正向否定预查（negative assert），在任何不匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如“`Windows(?!95\|98\|NT\|2000)`”能匹配“`Windows3.1`”中的“`Windows`”，但不能匹配“`Windows2000`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始 |
| `(?<=pattern)` | 反向（look behind）肯定预查，与正向肯定预查类似，只是方向相反。例如，“`(?<=95\|98\|NT\|2000)Windows`”能匹配“`2000Windows`”中的“`Windows`”，但不能匹配“`3.1Windows`”中的“`Windows`”。 |
| `(?            | 反向否定预查，与正向否定预查类似，只是方向相反。例如“`(?”能匹配“`3.1Windows`”中的“`Windows`”，但不能匹配“`2000Windows`”中的“`Windows`”。 |
| `x\|y`         | 没有包围在()里，其范围是整个正则表达式。例如，“`z\|food`”能匹配“`z`”或“`food`”。“`(?:z\|f)ood`”则匹配“`zood`”或“`food`”。 |
| `[xyz]`        | 字符集合（character class）。匹配所包含的任意一个字符。例如，“`[abc]`”可以匹配“`plain`”中的“`a`”。特殊字符仅有反斜线\保持特殊含义，用于转义字符。其它特殊字符如星号、加号、各种括号等均作为普通字符。脱字符^如果出现在首位则表示负值字符集合；如果出现在字符串中间就仅作为普通字符。连字符 - 如果出现在字符串中间表示字符范围描述；如果如果出现在首位（或末尾）则仅作为普通字符。右方括号应转义出现，也可以作为首位字符出现。 |
| `[^xyz]`       | 排除型字符集合（negated character classes）。匹配未列出的任意字符。例如，“`[^abc]`”可以匹配“`plain`”中的“`plin`”。 |
| `[a-z]`        | 字符范围。匹配指定范围内的任意字符。例如，“`[a-z]`”可以匹配“`a`”到“`z`”范围内的任意小写字母字符。 |
| `[^a-z]`       | 排除型的字符范围。匹配任何不在指定范围内的任意字符。例如，“`[^a-z]`”可以匹配任何不在“`a`”到“`z`”范围内的任意字符。 |
| `[:name:]`     | 增加命名字符类（named character class）中的字符到表达式。只能用于**「方括号表达式」**。 |
| `[=elt=]`      | 增加当前locale下排序（collate）等价于字符“elt”的元素。例如，[=a=]可能会增加ä、á、à、ă、ắ、ằ、ẵ、ẳ、â、ấ、ầ、ẫ、ẩ、ǎ、å、ǻ、ä、ǟ、ã、ȧ、ǡ、ą、ā、ả、ȁ、ȃ、ạ、ặ、ậ、ḁ、ⱥ、ᶏ、ɐ、ɑ 。只能用于方括号表达式。 |
| `[.elt.]`      | 增加排序元素elt到表达式中。这是因为某些排序元素由多个字符组成。例如，29个字母表的西班牙语， "CH"作为单个字母排在字母C之后，因此会产生如此排序“cinco, credo, chispa”。只能用于方括号表达式。 |
| `\b`           | 匹配一个单词边界，也就是指单词和空格间的位置。例如，“`er\b`”可以匹配“`never`”中的“`er`”，但不能匹配“`verb`”中的“`er`”。 |
| `\B`           | 匹配非单词边界。“`er\B`”能匹配“`verb`”中的“`er`”，但不能匹配“`never`”中的“`er`”。 |
| `\cx`          | 匹配由x指明的控制字符。x的值必须为`A-Z`或`a-z`之一。否则，将c视为一个原义的“`c`”字符。控制字符的值等于x的值最低5比特（即对3210进制的余数）。例如，\cM匹配一个Control-M或回车符。\ca等效于\u0001, \cb等效于\u0002, 等等… |
| `\d`           | 匹配一个数字字符。等价于[0-9]。注意Unicode正则表达式会匹配全角数字字符。 |
| `\D`           | 匹配一个非数字字符。等价于[^0-9]。                           |
| `\f`           | 匹配一个换页符。等价于\x0c和\cL。                            |
| `\n`           | 匹配一个换行符。等价于\x0a和\cJ。                            |
| `\r`           | 匹配一个回车符。等价于\x0d和\cM。                            |
| `\s`           | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。注意Unicode正则表达式会匹配全角空格符。 |
| `\S`           | 匹配任何非空白字符。等价于[^ \f\n\r\t\v]。                   |
| `\t`           | 匹配一个制表符。等价于\x09和\cI。                            |
| `\v`           | 匹配一个垂直制表符。等价于\x0b和\cK。                        |
| `\w`           | 匹配包括下划线的任何单词字符。等价于“`[A-Za-z0-9_]`”。注意Unicode正则表达式会匹配中文字符。 |
| `\W`           | 匹配任何非单词字符。等价于“`[^A-Za-z0-9_]`”。                |
| `\xnn`         | 十六进制转义字符序列。匹配两个十六进制数字nn表示的字符。例如，“`\x41`”匹配“`A`”。“`\x041`”则等价于“`\x04&1`”。正则表达式中可以使用ASCII编码。. |
| `\num`         | 向后引用（back-reference）一个子字符串（substring），该子字符串与正则表达式的第num个用括号围起来的捕捉群（capture group）子表达式（subexpression）匹配。其中num是从1开始的十进制正整数，其上限可能是9、31、99，甚至无限。例如：“`(.)\1`”匹配两个连续的相同字符。 |
| `\n`           | 标识一个八进制转义值或一个向后引用。如果\n之前至少n个获取的子表达式，则n为向后引用。否则，如果n为八进制数字（0-7），则n为一个八进制转义值。 |
| `\nm`          | 3位八进制数字，标识一个八进制转义值或一个向后引用。如果\nm之前至少有nm个获得子表达式，则nm为向后引用。如果\nm之前至少有n个获取，则n为一个后跟文字m的向后引用。如果前面的条件都不满足，若n和m均为八进制数字（0-7），则\nm将匹配八进制转义值nm。 |
| `\nml`         | 如果n为八进制数字（0-3），且m和l均为八进制数字（0-7），则匹配八进制转义值nml。 |
| `\un`          | Unicode转义字符序列。其中n是一个用四个十六进制数字表示的Unicode字符。例如，\u00A9匹配著作权符号（©）。 |

**优先权**

| 优先权 | 符号                                  |
| :----- | :------------------------------------ |
| 最高   | `\`                                   |
| 高     | `()`、`(?:)`、`(?=)`、`[]`            |
| 中     | `*`、`+`、`?`、`{n}`、`{n,}`、`{n,m}` |
| 低     | `^`、`$`、中介字符                    |
| 次最低 | 串接，即相邻字符连接在一起            |
| 最低   | `\|`                                  |

# 任务超时退出的装饰器

我们日常在使用的各种网络请求库时都带有timeout参数，例如request库。这个参数可以使请求超时就不再继续了，直接抛出超时错误，避免等太久。

如果我们自己开发的方法也希望增加这个功能，该如何做呢？

方法很多，但最简单直接的是使用并发库futures，为了使用方便，我将其封装成了一个装饰器，代码如下：

```
import functools
from concurrent import futures

executor = futures.ThreadPoolExecutor(1)

def timeout(seconds):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            future = executor.submit(func, *args, **kw)
            return future.result(timeout=seconds)
        return wrapper
    return decorator
```

定义了以上函数，我们就有了一个超时结束的装饰器，下面可以测试一下：

```
import time

@timeout(1)
def task(a, b):
    time.sleep(1.2)
    return a+b

task(2, 3)
```

结果：

```
---------------------------------------------------------------------------
TimeoutError                              Traceback (most recent call last)
...
D:\Anaconda3\lib\concurrent\futures\_base.py in result(self, timeout)
    432                 return self.__get_result()
    433             else:
--> 434                 raise TimeoutError()
    435 
    436     def exception(self, timeout=None):

TimeoutError:
```

上面我们通过装饰器定义了函数的超时时间为1秒，通过睡眠模拟函数执行超过1秒时，成功的抛出了超时异常。

程序能够在超时时间内完成时：

```
@timeout(1)def task(a, b):    time.sleep(0.9)    return a+btask(2, 3)
```

结果：

```
5
```

可以看到，顺利的得到了结果。

这样我们就可以通过一个装饰器给任何函数增加超时时间，这个函数在规定时间内还处理不完就可以直接结束任务。

前面我将这个装饰器将所需的变量定义到了外部，其实我们还可以通过类装饰器进一步封装，代码如下：

```
import functools
from concurrent import futures

class timeout:
    __executor = futures.ThreadPoolExecutor(1)

    def __init__(self, seconds):
        self.seconds = seconds

    def __call__(self, func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            future = timeout.__executor.submit(func, *args, **kw)
            return future.result(timeout=self.seconds)
        return wrapper
```

经测试使用类装饰器能得到同样的效果。

> 注意：使用@functools.wraps的目的是因为被装饰的func函数元信息会被替换为wrapper函数的元信息，而@functools.wraps(func)将wrapper函数的元信息替换为func函数的元信息。最终虽然返回的是wrapper函数，元信息却依然是原有的func函数。
>
> 在函数式编程中，函数的返回值是函数对象被称为闭包。

## **日志记录**

如果我们需要记录部分函数的执行时间，函数执行前后打印一些日志，装饰器是一种很方便的选择。

代码如下：

```
import time
import functools
 
def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        res = func(*args, **kwargs)
        end = time.perf_counter()
        print(f'函数 {func.__name__} 耗时 {(end - start) * 1000} ms')
        return res
    return wrapper
```

装饰器 log 记录某个函数的运行时间，并返回其执行结果。

测试一下：

```
@log
def now():
    print('2021-7-1')
    
now()
```

结果：

```
2021-7-1函数 now 耗时 0.09933599994838005 ms
```

## **缓存**

如果经常调用一个函数，而且参数经常会产生重复，如果把结果缓存起来，下次调用同样参数时就会节省处理时间。

定义函数：

```
import math
import random
import time


def task(x):
    time.sleep(0.01)
    return round(math.log(x**3 / 15), 4)
```

执行：

```
%%time
for i in range(500):
    task(random.randrange(5, 10))
```

结果：

```
Wall time: 5.01 s
```

此时如果我们使用缓存的效果就会大不一样，实现缓存的装饰器有很多，我就不重复造轮子了，这里使用functools包下的LRU缓存：

```
from functools import lru_cache

@lru_cache()
def task(x):
    time.sleep(0.01)
    return round(math.log(x**3 / 15), 4)
```

执行：

```
%%time
for i in range(500):
    task(random.randrange(5, 10))
```

结果：

```
Wall time: 50 ms
```

## **约束某个函数的可执行次数**

如果我们希望程序中的某个函数在整个程序的生命周期中只执行一次或N次，可以写一个这样的装饰器：

```
import functools


class allow_count:
    def __init__(self, count):
        self.count = count
        self.i = 0

    def __call__(self, func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            if self.i >= self.count:
                return
            self.i += 1
            return func(*args, **kw)
        return wrapper
```

测试：

```
@allow_count(3)
def job(x):
    x += 1
    return x
for i in range(5):
    print(job(i))
```

结果：

```
1
2
3
None
None
```

### 矫正xml格式

```python
from xml.etree import ElementTree

def prettyXml(element, indent, newline, level=0):  # elemnt为传进来的Elment类，参数indent用于缩进，newline用于换行
    if element:  # 判断element是否有子元素
        if element.text == None or element.text.isspace():  # 如果element的text没有内容
            element.text = newline + indent * (level + 1)
        else:
            element.text = newline + indent * (level + 1) + element.text.strip() + newline + indent * (level + 1)
    temp = list(element)
    for subelement in temp:
        if temp.index(subelement) < (len(temp) - 1):
            subelement.tail = newline + indent * (level + 1)
        else:
            subelement.tail = newline + indent * level
        prettyXml(subelement, indent, newline, level=level + 1)


if __name__ == '__main__':

  tree = ElementTree.parse('students.xml')
  root = tree.getroot()
  prettyXml(root, '\t', '\n')
  tree.write('students.xml',encoding='utf-8',xml_declaration=True) 
```

