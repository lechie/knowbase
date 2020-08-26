# mkdir

>make directory

通过 mkdir 命令可以实现在指定位置创建文件夹或目录。
要创建文件夹或目录的用户必须对所创建的文件夹的父文件夹具有写权限，并且指定的目录名不能是当前目录中已有的目录。

## 语法
mkdir [选项] 目录...

## 参数
参数 | 说明
--|--
-m, --mode=模式|设定权限<模式> (类似 chmod)，而不是 rwxrwxrwx 减 umask
-p, --parents|可以是个路径字符串，例如 a/b/cd/efg。此时若路径中的某些目录尚不存在则自动创建，即一次可以建立多个目录; 
-v, --verbose|每次创建新目录都显示信息
--help|显示此帮助信息
--version|显示版本信息

## 示例
```
# 创建一个空目录 
mkdir dir1

# 递归创建多个目录 
mkdir -p dir2/dir3

# 创建权限为777的目录,创建后权限为rwxrwxrwx
mkdir -m 777 dir4

# 创建时输出信息
mkdir -v dir5

# 创建多个目录
mkdir dir6 dir7 dir8

# 创建多个目录，注意指令中","后不能有空格
mkdir -vp test/{a/,b/,c/{c1,c2},d/{d1,d2},e/f/{f1,f2}}
tree test/
```
