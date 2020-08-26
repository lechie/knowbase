# mv
> mv=move

mv 可以用来移动文件或者将文件改名
视mv命令中第二个参数类型的不同（是目标文件还是目标目录），mv命令将文件重命名或将其移至一个新的目录中。当第二个参数类型是文件时，mv命令完成文件重命名
当第二个参数是已存在的目录名称时，源文件或目录参数可以有多个，mv命令将各参数指定的源文件均移至目标目录中。
在跨文件系统移动文件时，mv先拷贝，再将原有文件删除，而链至该文件的链接也将丢失。

## 语法
mv [选项] 源文件或目录 目标文件或目录

## 参数
## 参数
参数 | 说明
--|--
--backup[=CONTROL]|后跟参数,该选项指定如果目标文件存在时的动作，共有四种备份策略。
-b |若需覆盖文件，则覆盖前先行备份。 
-f |force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
-i |若目标文件已经存在时，就会询问是否覆盖！
-u |若目标文件已经存在，且 source 比较新，才会更新(update)
-t |位置发生变化，这里第一个参数是目标路径，此时目标目录在前，源文件在后。

> --backup[=CONTROL]该选项指定如果目标文件存在时的动作，共有四种备份策略：
> 
> 1.CONTROL=none或off : 不备份
> 2.CONTROL=numbered或t：数字编号的备份
> 3.CONTROL=existing或nil：如果存在以数字编号的备份，则继续编号备份m+1...n：执行mv操作前已存在以数字编号的文件log2.txt.~1~，那么再次执行将产生log2.txt~2~，以次类推。如果之前没有以数字编号的文件，则使用下面讲到的简单备份。
> 4.CONTROL=simple或never：使用简单备份：在被覆盖前进行了简单备份，简单备份只能有一份，再次被覆盖时，简单备份也会被覆盖。

## 示例
```
# 文件改名
mv a.txt b.log

#移动文件
mv a.txt dir1/
mv a.txt /dir0/dir1/.

# 将文件log1.txt,log2.txt,log3.txt移动到目录dir3中
mv log1.txt log2.txt log3.txt dir3

mv -t dir3/. log1.txt log2.txt log3.txt 


# 将文件file1改名为file2，如果file2已经存在，则询问是否覆盖
mv -i file1.txt file2.txt

# 将文件file1改名为file2，即使file2存在，也是直接覆盖
mv -f file1.txt file2.txt

# 目录的移动
mv dir1 dir2 
# 如果目录dir2不存在，将目录dir1改名为dir2；否则，将dir1移动到dir2中。

 

# 移动当前文件夹下的所有文件到上一级目录
mv * ../

# 文件被覆盖前做简单备份，前面加参数-b
mv -b log1.txt log2.txt
# -b 不接受参数，mv会去读取环境变量VERSION_CONTROL来作为备份策略
