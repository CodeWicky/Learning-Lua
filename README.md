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

