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




