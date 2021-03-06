# touch

`touch` 一般用于新建一个不存在的文件，或者在使用make的时候可能会用到，用来修改文件时间戳。

`touch` 可更改文档或目录的日期时间，包括存取时间和更改时间。 

## 语法

```
touch [选项] 文件名...
```

## 参数
参数 | 等价参数 | 说明
--|--|--
-a | --time=atime或--time=access或--time=use | 只更改存取时间
-c | --no-create | 不建立任何文档
-d| | 使用指定的日期时间，而非现在的时间 `touch -d [[CC]YY]MMDD filename...`
-f| | 此参数将忽略不予处理，仅负责解决BSD版本touch指令的兼容性问题
-m | --time=mtime或--time=modify | 只更改变动时间
-r| | 把指定文档或目录的日期时间，统统设成和参考文档或目录的日期时间相同
-t | | 使用指定的日期时间，而非现在的时间 `touch -t [CC[YY]MMDDhhmm[.SS] filename...`
 
> -t 说明
> time规定为如下形式的十进制数:      
> ```
> [[CC]YY]MMDDhhmm[.SS]
> ```
> 其中 CCYY 组合为4位数年份，CC为年份中的前两位，YY为年数的后两位，只给后两位也可，如果不给出CC的值，则 touch 将把年数CCYY限定在1969--2068之内
> MM为月数，DD为天数，hh为小时数，mm为分钟数，SS为秒数．此处秒的设定范围是0--61，这样可以处理闰秒
> 由于系统的限制，早于1970年1月1日的时间是错误的。
  
## 示例

```
# 创建不存在的文件
touch a.log b.log

# 更新使 b.log的时间和 a.log时间戳相同
touch -r a.log b.log

# 如果 a.log不存在，则不创建文件
touch -c a.log

# 设定文件的时间戳
touch -t 202008011234.50 a.log
```
