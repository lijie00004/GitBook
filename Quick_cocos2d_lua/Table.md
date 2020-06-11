## Table

| 函数                                          | 描述                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| table.concat(table [, sep [, start [, end]]]) | 把start位置到end的所有元素连接，并以指定的分隔符(sep)隔开    |
| table.insert(table, [pos,] value)             | 在table的数组部分指定位置(pos)插入值为value的一个元素. pos参数可选, 默认为数组部分末尾 |
| table.remove(table [, pos])                   | 返回位于pos位置的元素.pos参数可选, 默认为table长度, 即从最后一个元素删起。 |
| table.sort(table [, comp])                    | 对给定的table进行升序排序                                    |
| table.move(a1,f,e,t[,a2])                     | 将表a中从索引f到e的元素移动到位置t上，或 把表a1中从下标f到e的value移动到表a2中，位置为a2下标从t开始 move 实际是拷贝了值，然后赋值给对应位置 |
| table.pack(...)                               | 返回一个索引从 1 开始的参数表 table，并会对这个 table 预定义一个字段 n，表示该表的长度。 t = table.pack("test", "param1", "param2", "param3") t.n |
| table.unpack()                                | 把列表转成一组返回值                                         |
|                                               |                                                              |
|                                               |                                                              |



## 队列

```lua
function listNew()
    return {first = 0, last = -1}
end

function pushFirst(list, value)
    local first = list.first - 1
    list.first = first
    list[first] = value
end

function pushLast(list, value)
    local last = list.last + 1
    list.last = last
    list[last] = value
end

function popFirst(list)
    local first = list.first
    if first > list.last then
        print("list is empty")
    end
    local value = list[first]
    list[first] = nil
    list.first = first + 1
    return value
end

function popLast(list)
    local last = list.last
    if list.first > last then
        print("list is empty")
    end
    local value = list[last]
    list[last] = nil
    list.last = last - 1
    return value
end
```

## 反向表

```lua
local days = {"Sunday", "Monday", "Tuesday", "Wednesday"}
local revDays = {} -- {"Sunday" = 1, "Monday" = 2, "Tuesday" = 3, "Wednesday" = 4}
for i,v in ipairs(days) do
    revDays[v] = i;
end
```

## 按顺序遍历表

```lua
function pairsByKeys(_table, sortFunction)
    local tempTable = {}
    for k,v in pairs(_table) do
    	tempTable[#tempTable + 1] = k
    end
    table.sort(tempTable, sortFunction)
    local index = 0
    return function()
        index = index + 1
        return tempTable[index], _table[tempTable[index]]
    end
end
```

## 元表

* 查看t1是否有元表，如果有，是否有__add元方法，如果有就调用
* 查看t2是否有元表，如果有，是否有__add元方法，如果有就调用
* 如果都没有就会报错

```lua
local mt = {}
mt.__add = function(t1, t2)
    local temp = {}
    for k,v in pairs(t1) do
        table.insert(temp, v)
    end
    for k,v in pairs(t2) do
        table.insert(temp, v)
    end
    return temp
end

local t1 = {1,2,3}
local t2 = {4,5}
setmetatable(t1, mt)
local t3 = t1 + t2
local st = "{"
for k,v in pairs(t3) do
    st = st..v..","
end
st = st.."}"
print(st) -- {1,2,3,4,5,}
```

| 符号         | 元方法     |
| ------------ | ---------- |
| +            | __add      |
| -            | __sub      |
| *            | __mul      |
| /            | __div      |
| %            | __mod      |
| 当函数调用   | __call     |
| 转化为字符串 | __tostring |
| 调用一个索引 | __index    |
| 给一个索引值 | __newindex |

## 类

```lua
Class = {x=0,y=0}
Class.__index = Class

function Class:new(x,y)
    local self = {}
    setmetatable(self, Class)
    self.x = x
    self.y = y
    return self
end

function Class:test()
    print(self.x,self.y)
end

function Class:plus()
    self.x = self.x + 1
    self.y = self.y + 1
end
```

## 继承

```lua
require 'Class'

SubClass = {z = 0}  
setmetatable(SubClass, Class)  
SubClass.__index = SubClass  
function SubClass:new(x,y,z)  
    local self = Class:new(x,y)
    setmetatable(self, SubClass)
    self.z= z
    return self  
end  

function SubClass:go()  
    self.x = self.x + 10  
end  

function SubClass:test()  
    print(self.x,self.y,self.z)  
end
```

