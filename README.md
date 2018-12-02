# Learning-Lua



随公司架构需要，即将在主工程中接入Lua实现动态化开发。故开始学习Lua基础语法，于此做简单记录，记录与OC之间的一些差异与联系，以便日后查看、复习。通篇内容学习自[RUNOOB.COM/lua](https://www.runoob.com/lua)。

---

## 基本语法

### 注释

```lua
--单行注释

--[[
多行注释
多行注释
多行注释
--]]
```

### 关键字

||||||||
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|and|break|do|else|elseif|end|false|
|for|function|if|in|local|nil|not|
|or|repeat|return|then|true|until|while|


### 全局变量

默认情况下，变量总认为是全局的。
全局变量不需要声明，给一个变量赋值后即创建了这个全局变量，访问一个没有初始化的全局变量也不会出错，将得到结果nil。想要删除一个全局变量只要赋值为nil即可。感觉上更像是变量早已声明过，不过是没有初始化。

> 当且仅当一个变量不等于nil时，这个变量即存在。

---

## 数据类型

Lua中有8个基本类型分别为：nil、boolean、number、string、userdata、function、thread和table。

|数据类型|描述|
|:--:|:--:|
|nil|这个最简单，只有值nil属于该类，表示一个无效值（在条件表达式中相当于false）|
|boolean|包含两个值：false和true|
|number|表示双精度类型的实浮点数|
|string|字符串由一对双引号或单引号来表示|
|function|由 C 或 Lua 编写的函数|
|userdata|表示任意存储在变量中的C数据结构|
|thread|表示执行的独立线路，用于执行协同程序|
|table|Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字或者是字符串。在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表|

### nil

所有未初始化的变量均为nil类型。同时，删除一个变量，可将该变量的值设为nil。

> 与nil判等时，应使用type(A) == "nil"进行判等。

### boolean

boolean 类型只有两个可选值：true（真） 和 false（假），Lua 把 false 和 nil 看作是"假"，其他的都为"真"。

### number

Lua 默认只有一种 number 类型 -- double（双精度）类型（默认类型可以修改 luaconf.h 里的定义）。

### string

字符串由一对双引号或单引号来表示。

```
string1 = "this is string1"
string2 = 'this is string2'
```

也可以用 2 个方括号 "[[]]" 来表示"一块"字符串。

```
html = [[
<html>
<head></head>
<body>
    <a href="http://www.runoob.com/">菜鸟教程</a>
</body>
</html>
]]
print(html)
```

在对一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字:

```
> print("2" + 6)
8.0
> print("2" + "6")
8.0
> print("2 + 6")
2 + 6
> print("-2e2" * "6")
-1200.0
> print("error" + 1)
stdin:1: attempt to perform arithmetic on a string value
stack traceback:
    stdin:1: in main chunk
    [C]: in ?
> 
```

字符串连接使用的是 .. ，如：

```
> print("a" .. 'b')
ab
> print(157 .. 428)
157428
```

使用 # 来计算字符串的长度，放在字符串前面，如下实例：

```
> len = "www.runoob.com"
> print(#len)
14
> print(#"www.runoob.com")
14
```

### table

在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。也可以在表里添加一些数据，直接初始化表:

```
-- 创建一个空的 table
local tbl1 = {}
 
-- 直接初始表
local tbl2 = {key = "apple", "pear", "orange", "grape"}
print(tbl2.key)
print(tbl2["key"])
```

- 直接通过构造表达式初始表的时候，可以指定一个键值。直接写键值。键值不可以是string或者number。取值时可以通过.key和["key"]两种方式取值。
- 初始化后赋值时可以使用string或者number。
- table作为数组使用是，角标从1开始。

示例代码：

```
a = {}
a["key"] = "value"
key = 10
a[key] = 22
a[key] = a[key] + 11
for k, v in pairs(a) do
    print(k .. " : " .. v)
end

-- 执行结果 --
key : value
10 : 33
```

### function

在 Lua 中，函数是被看作是"第一类值（First-Class Value）"，函数可以存在变量里:

```
function factorial1(n)
    if n == 0 then
        return 1
    else
        return n * factorial1(n - 1)
    end
end
print(factorial1(5))
factorial2 = factorial1
print(factorial2(5))

-- 执行结果 --
120
120
```

unction 可以以匿名函数（anonymous function）的方式通过参数传递:

```
function testFun(tab,fun)
    for k ,v in pairs(tab) do
        print(fun(k,v));
    end
end


tab={key1="val1",key2="val2"};
testFun(tab,
function(key,val)--匿名函数
    return key.."="..val;
end
);

-- 执行结果 --
key1 = val1
key2 = val2
```

### thread 

在 Lua 里，最主要的线程是协同程序（coroutine）。它跟线程（thread）差不多，拥有自己独立的栈、局部变量和指令指针，可以跟其他协同程序共享全局变量和其他大部分东西。

线程跟协程的区别：线程可以同时多个运行，而协程任意时刻只能运行一个，并且处于运行状态的协程只有被挂起（suspend）时才会暂停。

### userdata

userdata 是一种用户自定义数据，用于表示一种由应用程序或 C/C++ 语言库所创建的类型，可以将任意 C/C++ 的任意数据类型的数据（通常是 struct 和 指针）存储到 Lua 变量中调用。

---

## 变量

变量在使用前，必须在代码中进行声明，即创建该变量。

编译程序执行代码之前编译器需要知道如何给语句变量开辟存储区，用于存储变量的值。

Lua 变量有三种类型：全局变量、局部变量、表中的域。

Lua 中的变量全是全局变量，那怕是语句块或是函数里，除非用 local 显式声明为局部变量。

局部变量的作用域为从声明位置开始到所在语句块结束。

变量的默认值均为 nil。

```
-- test.lua 文件脚本
a = 5               -- 全局变量
local b = 5         -- 局部变量

function joke()
    c = 5           -- 全局变量
    local d = 6     -- 局部变量
end

joke()
print(c,d)          --> 5 nil

do 
    local a = 6     -- 局部变量
    b = 6           -- 对局部变量重新赋值
    print(a,b);     --> 6 6
end

print(a,b)      --> 5 6
```

### 赋值

```
-- 基础赋值
a = "hello" .. "world"

-- 依次赋值（遇到赋值语句Lua会先计算右边所有的值然后再执行赋值操作，所以我们可以这样进行交换变量的值）
x, y = y, x                     -- swap 'x' for 'y'

-- 个数不同
--[[
a. 变量个数 > 值的个数             按变量个数补足nil
b. 变量个数 < 值的个数             多余的值会被忽略
--]]
```

---

## 循环

### while

```
while(<条件>)
do
   <执行体>
end
```

### for

```
-- var 从 exp1 变化到 exp2，每次变化以 exp3 为步长递增 var，并执行一次 "执行体"。exp3 是可选的，如果不指定，默认为1。
for var=exp1,exp2,exp3 do  
    <执行体>  
end  
```

针对for，我们看一组代码：

```
function f(x)  
    print("function")  
    return x*2   
end  
for i=1,f(5) do 
print(i)  
end  
```

for的三个表达式在循环开始前一次性求值，以后不再进行求值。比如上面的f(x)只会在循环开始前执行一次，其结果用在后面的循环中。

for同时支持泛型循环，类似于oc中的快速遍历：

```
days = {"Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"}  
for i,v in ipairs(days) do  print(v) end   
```

### repeat...until

等同于do...while。

### break
跳出循环。

---

## 流程控制

```
if (0)
then
	print("0 为 true")
elseif (nil) 
then
	print("nil 为 false") 
else 
	print("只有nil和false是false，其他均为true")
end
```

---

## 函数

```
<作用域> function <函数名>( <参数1>, <参数2>, <参数3>..., <参数n>)
    <函数体>
    return <返回值>
end
```

lua可返回多个返回值。

```
function maximum (a)
    local mi = 1             -- 最大值索引
    local m = a[mi]          -- 最大值
    for i,val in ipairs(a) do
       if val > m then
           mi = i
           m = val
       end
    end
    return m, mi
end

print(maximum({8,10,23,12,5}))

-- 执行结果 --
23    3
```

lua也支持可变参数。

```
function average(...)
   result = 0
   local arg={...}    --> arg 为一个表，局部变量
   for i,v in ipairs(arg) do
      result = result + v
   end
   print("总共传入 " .. #arg .. " 个数")
   return result/#arg
end

print("平均值为",average(10,5,3,4,5,6))
```

```
function fwrite(fmt, ...)  ---> 固定的参数fmt
    return io.write(string.format(fmt, ...))     
end

fwrite("runoob\n")       --->fmt = "runoob", 没有变长参数。  
fwrite("%d%d\n", 1, 2)   --->fmt = "%d%d", 变长参数为 1 和 2
```

select函数

通常在遍历变长参数的时候只需要使用 {…}，然而变长参数可能会包含一些 nil，那么就可以用 select 函数来访问变长参数了：select('#', …) 或者 select(n, …)

select('#', …) 返回可变参数的长度
select(n, …) 用于访问 n 到 select('#',…) 的参数
调用select时，必须传入一个固定实参selector(选择开关)和一系列变长参数。如果selector为数字n,示例如下：

```
function foo(...)
	for i = 1,select('#',...) do
		print(select(i,...))
	end
end

foo(1,2,3,4,5)
```

---

## 运算符

### 算数运算符

|加|减/负|乘|除|余|幂|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|+|-|*|/|%|^|

### 关系运算符

|等于|不等于|大于|大于等于|小于|小于等于|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|==|~=|>|>=|<|<=|

### 逻辑运算符

|逻辑与|逻辑或|逻辑非|
|:--:|:--:|:--:|
|and|or|not|

### 其他运算符

|连接(字符串)|长度(字符串或表)|
|:--:|:--:|
|..|#|

---

## 字符串

### 表达方式

字符串或串(String)是由数字、字母、下划线组成的一串字符。

Lua 语言中字符串可以使用以下三种方式来表示：

单引号间的一串字符。
双引号间的一串字符。
[[和]]间的一串字符。
以上三种方式的字符串实例如下：

```
string1 = "Lua"
print("\"字符串 1 是\"",string1)
string2 = 'runoob.com'
print("字符串 2 是",string2)

string3 = [["Lua 教程"]]
print("字符串 3 是",string3)

-- 输出结果 --
"字符串 1 是"    Lua
字符串 2 是    runoob.com
字符串 3 是    "Lua 教程"
```

### 字符串操作

|方法|用途|
|:--|:--|
|string.upper(argument)|字符串全部转为大写字母|
|string.lower(argument)|字符串全部转为小写字母|
|string.gsub(mainString,findString,replaceString,num)|在字符串中替换,mainString为要替换的字符串， findString 为被替换的字符，replaceString 要替换的字符，num 替换次数（可以忽略，则全部替换）|
|string.find (str, substr, [init, [end]])|在一个指定的目标字符串中搜索指定的内容(第三个参数为索引),返回其具体位置。不存在则返回 nil|
|string.reverse(arg)|字符串反转(调用者需为字符串或string，实际翻转对象为传入参数，与调用者无关)|
|string.format(...)|返回一个类似printf的格式化字符串|
|string.char(arg) 和 string.byte(arg[,int])|char 将整型数字转成字符并连接， byte 转换字符为整数值(可以指定某个字符，默认第一个字符)|
|string.len(arg)|计算字符串长度|
|string.rep(string, n)|返回字符串string的n个拷贝|
|..|链接两个字符串|
|string.gmatch(str, pattern)|回一个迭代器函数，每一次调用这个函数，返回一个在字符串 str 找到的下一个符合 pattern 描述的子串。如果参数 pattern 描述的字符串没有找到，迭代函数返回nil|
|string.match(str, pattern, init)|string.match()只寻找源字串str中的第一个配对. 参数init可选, 指定搜寻过程的起点, 默认为1。 在成功配对时, 函数将返回配对表达式中的所有捕获结果; 如果没有设置捕获标记, 则返回整个配对字符串. 当没有成功的配对时, 返回nil|

### 示例

```
-- 字符转换
-- 转换第一个字符
print(string.byte("Lua"))
-- 转换第三个字符
print(string.byte("Lua",3))
-- 转换末尾第一个字符
print(string.byte("Lua",-1))
-- 第二个字符
print(string.byte("Lua",2))
-- 转换末尾第二个字符
print(string.byte("Lua",-2))

-- 整数 ASCII 码转换为字符
print(string.char(97))


-- 字符串复制 2 次
repeatedString = string.rep(string2,2)
print(repeatedString)

-- 匹配模式
s = "Deadline is 30/05/1999, firm"
date = "%d%d/%d%d/%d%d%d%d"
print(string.sub(s, string.find(s, date)))    --> 30/05/1999

```

--- 

## 数组

用表实现即可。

---

## 迭代器

迭代器（iterator）是一种对象，它能够用来遍历标准模板库容器中的部分或全部元素，每个迭代器对象代表容器中的确定的地址。

在Lua中迭代器是一种支持指针类型的结构，它可以遍历集合的每一个元素。

### pairs()

遍历当前表

### ipairs()

遍历当前表中的数组（非nil元素）

### 实现一个迭代器

```
function square(iteratorMaxCount,currentNumber)
   if currentNumber<iteratorMaxCount
   then
      currentNumber = currentNumber+1
   return currentNumber, currentNumber*currentNumber
   end
end

for i,n in square,3,0
do
   print(i,n)
end
```

```
array = {"Lua", "Tutorial"}

function elementIterator (collection)
   local index = 0
   local count = #collection
   -- 闭包函数
   return function ()
      index = index + 1
      if index <= count
      then
         --  返回迭代器的当前元素
         return collection[index]
      end
   end
end

for element in elementIterator(array)
do
   print(element)
end
```

---

## table

### concat函数

```
-- 函数原型 table.concat (table [, sep [, start [, end]]])

a = {key = 1,key2 = 2,3,4,5,6}
print((a,1,2,3))

-- 运行结果 --
415
```

### insert

```
-- 函数原型 table.insert (table, [pos,] value):

function printT(a) 
	print("tbl length = ",#a)
	for k,v in pairs(a) do
		print(k,v)
	end
end


a = {key = 1}
table.insert(a, 1,"a")


printT(a)

-- 运行结果 --
tbl length = 	1
1	a
key	1

```

### remove函数

```
-- 函数原型 table.remove (table [, pos])


function printT(a) 
	print("tbl length = ",#a)
	for k,v in pairs(a) do
		print(k,v)
	end
end


a = {key = 1,1,2,2,3,}
table.remove(a, 1)


printT(a)

-- 运行结果 --
tbl length = 	3
1	2
2	2
3	3
key	1

```

### sort函数

```
-- 函数原型 table.sort (table [, comp])

fruits = {"banana","orange","apple","grapes"}
print("排序前")
for k,v in ipairs(fruits) do
	print(k,v)
end

table.sort(fruits)
print("排序后")
for k,v in ipairs(fruits) do
	print(k,v)
end

-- 运行结果 --
1	banana
2	orange
3	apple
4	grapes
排序后
1	apple
2	banana
3	grapes
4	orange
```

---

## 模块

模块类似于一个封装库，从 Lua 5.1 开始，Lua 加入了标准的模块管理机制，可以把一些公用的代码放在一个文件里，以 API 接口的形式在其他地方调用，有利于代码的重用和降低代码耦合度。形式上，更像是OC中类的概念。

Lua 的模块是由变量、函数等已知元素组成的 table，因此创建一个模块很简单，就是创建一个 table，然后把需要导出的常量、函数放入其中，最后返回这个 table 就行。以下为创建自定义模块 module.lua，文件代码格式如下：

```
-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "这是一个常量"
 
-- 定义一个函数
function module.func1()
    io.write("这是一个公有函数！\n")
end
 
local function func2()
    print("这是一个私有函数！")
end
 
function module.func3()
    func2()
end
 
return module
```

### 使用module

```
-- test_module.lua 文件
-- module 模块为上文提到到 module.lua
require("module")
 
print(module.constant)
 
module.func3()
```

--- 

## 元表

元表提供了我们改变表行为的能力。

我们可以通过`setmetatable`为表设置一个元表，在原表中我们通过修改相关函数实现来达到修改表行为的目的。我们还可以通过`getmetatable`获取表关联的元表。

### __index/__newindex

在表中，如果我们取一个表中不存在的键的值会直接返回nil，如果我们存一个表中不存在的键值，会直接创建一个新的键。

现在我们想实现这样一个功能，取值是，如果当前表中没有对应的键值，去另一张表中取值，下文中我们姑且称为备援表。赋值时，先检测表中存不存在当前键值，若存在直接更新，若不存在，先检测备援表中是否存在当前键值，若存在，更新备援表中的值，若不存在则在备援表中新建一个键值。

这个需求我们就可以通过`__index`和`__newindex`。

```
superTbl = {a = 1}
superTbl.say = function (a)
print("para = ",a)
end
superTbl.logA = function (a)
print("come to logA")
a.say(a.a)
end
metaTbl = {__index = superTbl,__newindex = superTbl}
childTbl = {b = 2}
childTbl = setmetatable(childTbl, metaTbl)
childTbl.super = superTbl
print("a = ",childTbl.a)
print("b = ",childTbl.b)
childTbl.say = function (a) 
print("override para = ",a)
end
childTbl.say(2)
childTbl.super.say(3)
print("before")
printT(childTbl)
childTbl.a = 3
print("after")
printT(childTbl)
childTbl.super.logA(superTbl)
```

上述代码中，我们则实现了这个需求。当然此处`__index`和`__newindex`中对应的表可以是不同的表。`__index`对应的是取值的备援表，`__newindex`对应的是更新值时的备援表。

借助这个属性，结合module的概念，我们大概可以实现一个类的继承关系。

### 运算操作

|__add|__sub|__mul|__div|__mod|__unm|__concat|__eq|__lt|__le|
|:--:|:--:|--:|:--:|--:|:--:|--:|:--:|--:|:--:|
|+|-|*|/|%|-|..|==|<|<=|

我们也可以自定义表的运算操作符。上述表中使我们支持的运算操作符。我们来看一个修改__add运算符的简单的示例:

```
-- 计算表中最大值，table.maxn在Lua5.2以上版本中已无法使用
-- 自定义计算表中最大键值函数 table_maxn，即计算表的元素个数
function table_maxn(t)
    local mn = 0
    for k, v in pairs(t) do
        if mn < k then
            mn = k
        end
    end
    return mn
end

-- 两表相加操作
mytable = setmetatable({ 1, 2, 3 }, {
  __add = function(mytable, newtable)
    for i = 1, table_maxn(newtable) do
      table.insert(mytable, table_maxn(mytable)+1,newtable[i])
    end
    return mytable
  end
})

secondtable = {4,5,6}

mytable = mytable + secondtable
    for k,v in ipairs(mytable) do
print(k,v)
end
```

### __call

__call 元方法在 Lua 调用一个值时调用。以下实例演示了计算表中元素的和：

```
-- 计算表中最大值，table.maxn在Lua5.2以上版本中已无法使用
-- 自定义计算表中最大键值函数 table_maxn，即计算表的元素个数
function table_maxn(t)
    local mn = 0
    for k, v in pairs(t) do
        if mn < k then
            mn = k
        end
    end
    return mn
end

-- 定义元方法__call
mytable = setmetatable({10}, {
  __call = function(mytable, newtable)
    sum = 0
    for i = 1, table_maxn(mytable) do
        sum = sum + mytable[i]
    end
    for i = 1, table_maxn(newtable) do
        sum = sum + newtable[i]
    end
    return sum
  end
})
newtable = {10,20,30}
print(mytable(newtable))
```

### __tostring 元方法

__tostring 元方法用于修改表的输出行为。以下实例我们自定义了表的输出内容：

```
mytable = setmetatable({ 10, 20, 30 }, {
  __tostring = function(mytable)
    sum = 0
    for k, v in pairs(mytable) do
        sum = sum + v
    end
    return "表所有元素的和为 " .. sum
  end
})
print(mytable)
```

---

## 协同

> 工程中未使用，待补

---

## 文件I/O

> 工程中未使用，待补

---

## 错误处理

> 工程中未使用，待补

---

## Debug

> 工程中未使用，待补

---

## 垃圾回收

> 工程中未使用，待补

---

## 面向对象

我们知道，对象由属性和方法组成。LUA中最基本的结构是table，所以需要用table来描述对象的属性。

lua中的function可以用来表示方法。那么LUA中的类可以通过table + function模拟出来。

至于继承，可以通过metetable模拟出来（不推荐用，只模拟最基本的对象大部分时间够用了）。

Lua中的表不仅在某种意义上是一种对象。像对象一样，表也有状态（成员变量）；也有与对象的值独立的本性，特别是拥有两个不同值的对象（table）代表两个不同的对象；一个对象在不同的时候也可以有不同的值，但他始终是一个对象；与对象类似，表的生命周期与其由什么创建、在哪创建没有关系。对象有他们的成员函数，表也有：

```
Account = {balance = 0}
function Account.withdraw (v)
    Account.balance = Account.balance - v
end
```

### 一个简单实例

```
-- Meta class
Rectangle = {area = 0, length = 0, breadth = 0}

-- 派生类的方法 new
function Rectangle:new (o,length,breadth)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  self.length = length or 0
  self.breadth = breadth or 0
  self.area = length*breadth;
  return o
end

-- 派生类的方法 printArea
function Rectangle:printArea ()
  print("矩形面积为 ",self.area)
end
```

### 创建对象

```
r = Rectangle:new(nil,10,20)
```

### 访问属性

```
print(r.length)
```

### 访问成员函数

```
r:printArea()
```

`.`与`:`的区别是：

> :是个语法糖，调用的函数会自动传递参数self

eg:

```
local a = {x = 0}
function a.foo(self, a)
self.x = a
end
function a:foo2(a)
self.x = a
end
--调用时：
a.foo(a, 2)
a.foo2(2)
```

上述两个操作是等价的，用:时就省去了定义和调用时需要额外添加self用来指代自身的麻烦

### 完整实例

```
-- Meta class
Shape = {area = 0}

-- 基础类方法 new
function Shape:new (o,side)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  side = side or 0
  self.area = side*side;
  return o
end

-- 基础类方法 printArea
function Shape:printArea ()
  print("面积为 ",self.area)
end

-- 创建对象
myshape = Shape:new(nil,10)

myshape:printArea()
```

### Lua 继承

继承是指一个对象直接使用另一对象的属性和方法。可用于扩展基础类的属性和方法。

```
 -- Meta class
Shape = {area = 0}
-- 基础类方法 new
function Shape:new (o,side)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  side = side or 0
  self.area = side*side;
  return o
end
-- 基础类方法 printArea
function Shape:printArea ()
  print("面积为 ",self.area)
end

-- 创建对象
myshape = Shape:new(nil,10)
myshape:printArea()

Square = Shape:new()
-- 派生类方法 new
function Square:new (o,side)
  o = o or Shape:new(o,side)
  setmetatable(o, self)
  self.__index = self
  return o
end

-- 派生类方法 printArea
function Square:printArea ()
  print("正方形面积为 ",self.area)
end

-- 创建对象
mysquare = Square:new(nil,10)
mysquare:printArea()

Rectangle = Shape:new()
-- 派生类方法 new
function Rectangle:new (o,length,breadth)
  o = o or Shape:new(o)
  setmetatable(o, self)
  self.__index = self
  self.area = length * breadth
  return o
end

-- 派生类方法 printArea
function Rectangle:printArea ()
  print("矩形面积为 ",self.area)
end

-- 创建对象
myrectangle = Rectangle:new(nil,10,20)
myrectangle:printArea()
```



