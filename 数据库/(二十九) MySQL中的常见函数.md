[TOC]

# 单行函数

## 大小写控制函数

LOWER(str) , UPPER(str)

## 字符处理函数

1. concat（str1,str2, str3,...)
2. substr(str, pos, len)
3. length(str)
4. instr(str, substr) 获取位置
5. Lpan(str, len, padstr)/Rpad(str, len, padstr)  左右填充字符
6. trim(str), ltrim(str), rtrim(str)
7. replace(str, from_str, to_str)

## 数字函数

1. ROUND(arg1, arg2): 四舍五入指定小数的值
2. round(arg1): 四舍五入保留整数
3. trunc(arg1, arg2): 截断指定小数的值, 不做四舍五入处理
4. mod(arg1, arg2): 取余

## 日期函数

1. sysdate() 或者 now() 返回当前系统时间, 格式为YYYY-MM-DD hh-mm-ss
2. curdate() 返回系统当前日期, 不返回时间
3. curtime() 返回当前系统中的时间, 不返回日期
4. dayofmonth(date) 计算日期d是本月的第几天
5. dayofweek(date): date今天是星期几, 1是星期日
6. dayofyear(date): 返回指定年份的天数
7. dayname(date): 返回date日期是星期几
8. last_day(date): 返回date日期当月的最后一天

## 转换函数

1. date_FORMAT(date, format): 将日期转换成字符串(类似Oracle中的to_char)
2. str_to_date(str,format) 将字符串转换成日期

![1564974507602](https://raw.githubusercontent.com/jssda/picbed/master/1564974507602.png)

## 通用函数

1. IFNULL(exp1, exp2): 类似Oracle中的NVL() 函数. 如果exp1为空, 则返回exp2. 如果不为空, 返回exp1
2. NULLIF(exp1, exp2), 如果exp1和exp2相等, 则返回null, 否则返回exp1
3. if(exp1,exp2,exp3): 相当于Oracle的NVL2()函数. 返回第一个为真或者不为null的表达式
4. coalesce(value, …)  返回第一个不为null的value

## 条件函数

cast when then else end

