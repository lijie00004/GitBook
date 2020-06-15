* `print(debug.traceback("Stack trace"))`
* 将 `nil` 赋值给全局变量，相当于将其删除
* `false nil` 为假，其它都为真，零和空字符串也视为真
* `if not x then x = v end` 如果没初始化，初始化
* `(x>y) and x or y` 求最大值，`(x>y) == true return x`
* `and or` 返回的是操作数
* `not` 运算符永远返回 `Boolean` 值
* 除法取整(//), 向负无穷取整, `9 // 2 = 4` `-9 // 2 = -5`
* 保留两位小数 `x - x % 0.01`
* 平方根 `x^2` 立方根 `x^(1/2)`
* 不等于 `~=`
* 长度操作符 `#"good"`
* 连接符 `"hello" .. "world"`
* 调用表值 `table.name` or `table["name"]`
* 冒号: 默认会接受self参数；而点号，默认不会接受self参数
* 必须定义和调用都用冒号，才会有self，否则只能手动传入`class.test(class)`
* `"0"` 和 `0` 其对应的值是不一样的，在表中
* `2` 和 `2.0` 在表中值是一样的
* 创建表:
  * `days = {"Sunday", "Monday"}`
  * `days = {[1] = "Sunday", [2] = "Monday"}`
  * `a = {x = 10, y = 20}`
  * `a = {["x"] = 10, ["y"] = 20}`
* 表的长度#，只能获取索引为连续数字的长度，如果不连续，只能获取到连续的地方，其他地方被舍弃
* `for k,v in pairs(table_name) do print(k,v) end` 遍历表中的键值对，每次遍历的顺序都是随机的
* `for i,v in ipairs(table_name) do print(i,v) end` 遍历列表({10,"hellow", 12}) 按顺序遍历
* `for i=1,#t do print(i, t[i]) end`
* `zip = company?.director?.address` 如果 company 不存在，返回 nil, 如果 director 不存在，返回 nil
* `E = {} zip = (((company or E).director or E).address or E).zipcode`
* `return "a", "b"` 函数可以返回多个值
* `function add(...) for _, v in ipairs{...} do end end` 可变参数数量
* `if while for end`  local 在外部是访问不到的
* `repeat local a = "a"; until a < 0;` 至少会执行一次，局部变量可在条件判断里访问
* `for var = exp1, exp2, exp3`  exp3 步长
* `require("filePath.fileName")`
* `getmetatable(_table)` 获取元表
* `setmetatable(_table, _table_1)` 设置元表
* `tonumber(val)`
* `tostring(val)`

---

## Math

| 函数名              | 描述                                       |
| ------------------- | ------------------------------------------ |
| math.random()       | 返回一个在[0,1)伪随机实数                  |
| math.random(n)      | 参数 n 是整型值，返回一个在[1,n]伪随机整数 |
| math.random(l,u)    | 参数 l u 是整型值，返回 [l, u]伪随机整数   |
| math.floor(num)     | 向负无穷取整                               |
| math.ceil(num)      | 向正无穷取整                               |
| math.modf(num)      | 向零取整，返回二个值，整数部分和小数部分   |
| math.tointeger(any) | 转换为整型值, string 也行                  |

## String

| 函数                                              | 描述                                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| string.len(str)                                   | 返回字符串的长度 等价 #s                                     |
| string.rep(s, n)                                  | 返回将字符串 s 重复 n 次的结果                               |
| string.reverse(n)                                 | 字符串翻转                                                   |
| string.lower(s)                                   | 字符串小写                                                   |
| string.upper(s)                                   | 字符串大写                                                   |
| string.sub(s, i, j)                               | 提取字符串 sub(s, 2, -2)                                     |
| string.format(s, d)                               | 字符串格式化和将数值输出为字符串                             |
| string.find("hello", "el", index, bookean)        | 返回开始和结束位置，找不到返回 nil，index 是查找的起始位置，Boolean 是否开启简单搜索 |
| string.gsub("hello", "l", ".")                    | 替代字符，返回新字符和替换次数                               |
| string.match("Today is 17/7/1990", "%d+/%d+/%d+") | 匹配字符串                                                   |

* d 十进制整数 x 十六进制整数 f 浮点数 s 字符串
* `string.format("x = %d y = %d", 10, 20)`
* `string.format("pi = %.4f", math.pi)`
* `string.format("%02d%02d", 5, 11)`
* `string.sub(s, i, j)` 等价 `s: sub(i, j)`
* `string.find("a [word]", "[")` 报错, 因为"["在模式中具有特殊含义
* `string.find("a [word]", "[", 1, true)` 开启简单模式，把"["当作简单字符串

| 字符 | 含义                                             |
| ---- | ------------------------------------------------ |
| .    | 任意字符                                         |
| %a   | 字母                                             |
| %d   | 数字                                             |
| %l   | 小写字母                                         |
| %p   | 标点符号                                         |
| %s   | 空白字符                                         |
| %u   | 大写字母                                         |
| %w   | 字母和数字                                       |
| +    | 重复一次或多次 -- 总是获取与模式相匹配的最长序列 |
| *    | 重复零次或多次                                   |
| -    | 重复零次或多次(最小匹配)                         |
| ？   | 可选(出现零次或一次)                             |

* 这些类的大写形式表示类的补集，如 %A 代表任意非字母的字符
* `string.match("Today is 17/7/1990", "%d+/%d+/%d+")`
* `string.match("DeadLine is 30/05/1999, firm", "%d%d/%d%d/%d%d%d%d")`
* `string.gsub("hello, up-down!", "%A", ".")`

## 时间和日期

* `os.time()` 当前秒数，和时区关联

* `os.time(table)` 返回从1970到参数时间之差，单位秒数，和时区关联

* table的几个字段中year, month, day是必填的，而hour, min, sec, isdst是可选的，isdst 是否开启夏令时

* `os.time({year=2008,month=8,day=8,hour=20,min=0,sec=0});` 值为 0

* 使用上面返回nil 因为返回负数。所以就返回了nil，可以在day上+1

* hour, min, sec这几个值的范围不一定是正常的时间，甚至可以是负数，比如时间08:62:-10就代表了09:01:50

  | 符号 | 含义                            |
  | ---- | ------------------------------- |
  | %a   | 星期几的简写(Wed)               |
  | %A   | 星期几的全名(Wednesday)         |
  | %b   | 月份的简写(Sep)                 |
  | %B   | 月份的全名(September)           |
  | %c   | 日期和时间(09/16/98 23：48：10) |
  | *t   | 表                              |

* 参数1 表示形式(格式化字符串) 参数2 秒数，可选(默认当前时间)

* `os.date("*t", 906000490)` 返回一个表 {year = 1998, month = 9...}

* `os.date("a %A in %B")` a Tuesday in May

* `os.date("%d/%m/%Y")`16/09/1998

* `os.date()` 默认使用%c格式

* `local t = os.date("*t")` 获取时间表

* `os.date("%Y/%m/%d", os.time(t))` 2015/08/18

* `t.day = t.day + 40` 增减天数，月份都可以

* `os.date("%Y/%m/%d", os.time(t))` 2015/09/27

* 月份加减后，会自动转换正确日期，如三月加一个月会进一成五月一日，所以月份加减后，会导致不是原先日期

> local t_1 = os.time({year=2012,month=1,day=12})
> local t_2 = os.time({year=2011,month=1,day=12})
> local d = os.difftime(t_2, t_1) -- 两个时间之间的差值，有正负

> local T = {year=2000,month=1,day=1,hour=0}
> T.sec = 501336000
> os.date("%d/%m/%Y", os.time(T)) -- 20/11/2015

## 闭包

```lua
function newCounter()
    local count = 0;
    return function()
        count = count + 1;
        return count;
    end
end
c1 = newCounter();
print(c1()) -- 1
print(c1()) -- 2
```