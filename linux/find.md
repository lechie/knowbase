# find

find命令在目录结构中搜索文件，并执行指定的操作。

即使系统中含有网络文件系统( NFS)，find命令在该文件系统中同样有效，只你具有相应的权限。

在运行一个非常消耗资源的find命令时，很多人都倾向于把它放在后台执行，因为遍历一个大的文件系统可能会花费很长的时间(这里是指30G字节以上的文件系统)。


## 语法
find pathname -options [-print -exec -ok ...]

## 参数
参数| 说明
--|--
pathname|find命令所查找的目录路径。例如用.来表示当前目录，用/来表示系统根目录。 
-print|find命令将匹配的文件输出到标准输出。 
-exec|find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' {  } \;，注意{   }和\；之间的空格。 
-ok|和-exec的作用相同，只不过以一种更为安全的模式来执行该参数所给出的shell命令，在执行每一个命令之前，都会给出提示，让用户来确定是否执行。

## 命令选项
命令选项|说明
--|--
-name   |按照文件名查找文件。
-perm   |按照文件权限来查找文件。
-prune  |使用这一选项可以使find命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被find命令忽略。
-user   |按照文件属主来查找文件。
-group  |按照文件所属的组来查找文件。
-nogroup  |查找无有效所属组的文件，即该文件所属的组在/etc/groups中不存在。
-nouser   |查找无有效属主的文件，即该文件的属主在/etc/passwd中不存在。
-newer file1 ! file2  |查找更改时间比文件file1新但比文件file2旧的文件。
-type  |查找某一类型的文件，诸如：<br>b - 块设备文件<br>d - 目录<br>c - 字符设备文件<br>p - 管道文件<br>l - 符号链接文件<br>f - 普通文件
-size n：[c] |查找文件长度为n块的文件，带有c时表示文件长度以字节计。-depth：在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找。
-fstype：|查找位于某一类型文件系统中的文件，这些文件系统类型通常可以在配置文件/etc/fstab中找到，该配置文件中包含了本系统中有关文件系统的信息。
-mount：|在查找文件时不跨越文件系统mount点。
-follow：|如果find命令遇到符号链接文件，就跟踪至链接所指向的文件。
-cpio：|对匹配的文件使用cpio命令，将这些文件备份到磁带设备中。
-mtime/mmin/ctime/cmin/atime/amin -n +n  |按照文件的更改时间来查找文件

另外,还有如下几种用法：
参数|说明
--|--
-amin n |查找系统中最后N分钟访问的文件
-atime n  |查找系统中最后n*24小时访问的文件
-cmin n |查找系统中最后N分钟被改变文件状态的文件
-ctime n |查找系统中最后n*24小时被改变文件状态的文件
-mmin n   |查找系统中最后N分钟被改变文件数据的文件
-mtime n  |查找系统中最后n*24小时被改变文件数据的文件

> -n表示文件更改时间距现在n天以内，+n表示文件更改时间距现在n天以前。

## 示例
```
# 查找48小时内修改过的文件 
find -atime -2

# 在当前目录查找 以.log结尾的文件。 ". "代表当前目录 
find . -name "*.log"

# 查找/dir0/dir1/目录下 权限为 777的文件
find /dir0/dir1/ -perm 777

# 查找当目录，以.log结尾的普通文件 
find . -type f -name "*.log"

# 查找当前所有目录并排序
find . -type d | sort

# 查找当前目录大于1K的文件 
find . -size +1000c -print
```

# 高级操作示例

## 使用name选项

文件名选项是find命令最常用的选项，要么单独使用该选项，要么和其他选项一起使用。

可以使用某种文件名模式来匹配文件，记住要用引号将文件名模式引起来。

```
# 在根目录$HOME中查找文件名符合*.log的文件，使用~作为 'pathname'参数，波浪号~代表了$HOME目录。
find ~ -name "*.log" -print

# 在当前目录及子目录中查找所有的*.log文件 
find . -name "*.log" -print  

# 在当前目录及子目录中查找文件名以一个大写字母开头的文件  
find . -name "[A-Z]*" -print  

# 在/etc目录中查找文件名以host开头的文件  
find /etc -name "host*" -print  

# 查找$HOME目录中的文件 
find ~ -name "*" -print 或find . -print  

# 从根目录开始查找所有的文件
find / -name "*" -print  

# 如果想在当前目录查找文件名以一个个小写字母开头，最后是4到9加上.log结束的文件：  
find . -name "[a-z]*[4-9].log" -print
```

## 用perm选项

按照文件权限模式用-perm选项,按文件权限模式来查找文件的话。最好使用二进制的权限表示法。  

```
# 如在当前目录下查找文件权限位为755的文件，即文件属主可以读、写、执行，其他用户可以读、执行的文件
find . -perm 755 -print

# 还有一种表达方法：在数字前加-，表示至少匹配，如-007就相当于权限至少007，-005相当于至少满足005
find . -perm -005
```

## 忽略某个目录：

如果在查找文件时希望忽略某个目录，因为你知道那个目录中没有你所要查找的文件，那么可以使用-prune选项来指出需要忽略的目录。

在使用-prune选项时要当心，因为如果你同时使用了-depth选项，那么-prune选项就会被find命令忽略。

```
# 在test目录下查找文件，但不希望在test/test3目录下查找
find test -path "test/test3" -prune -o -print


# 在test 目录下查找不在test4子目录之内的所有文件
find test -path "test/test4" -prune -o -print

# 避开多个文件夹:
find test \( -path test/test4 -o -path test/test3 \) -prune -o -print 

# 说明：圆括号表示表达式的结合。  \ 表示引用，即指示 shell 不对后面的字符作特殊解释，而留给 find 命令去解释其意义。  


# 查找某一确定文件，-name等选项加在-o 之后
find test \(-path test/test4 -o -path test/test3 \) -prune -o -name "*.log" -print
```
> -path "test" -prune -o -print 是 -path "test" -a -prune -o -print 的简写表达式按顺序求值, 
> -a 和 -o 都是短路求值，与 shell 的 && 和 || 类似
>
> 如果 -path "test" 为真，则求值 -prune , -prune 返回真，与逻辑表达式为真；
> 否则不求值 -prune，与逻辑表达式为假。
>
> 如果 -path "test" -a -prune 为假，则求值 -print ，-print返回真，或逻辑表达式为真；
> 否则不求值 -print，或逻辑表达式为真。 

这个表达式组合特例可以用伪码写为:
```
if -path "test" then  
-prune  
else  
-print  
```

## 使用user和nouser选项
```
#按文件属主查找文件：
#在$HOME目录中查找文件属主为peida的文件 
find ~ -user peida -print  

#在/etc目录下查找文件属主为peida的文件: 
find /etc -user peida -print  

# 在/home目录下查找属主帐户已经被删除的文件
find /home -nouser -print

# 说明：这样就能够找到那些属主在/etc/passwd文件中没有有效帐户的文件。在使用-nouser选项时，不必给出用户名
```

## 使用group和nogroup选项：
```
#就像user和nouser选项一样，针对文件所属于的用户组， find命令也具有同样的选项，为了在/apps目录下查找属于gem用户组的文件，可以用：  
find /apps -group gem -print  

#要查找没有有效所属用户组的所有文件，可以使用nogroup选项。下面的find命令从文件系统的根目录处查找这样的文件:
find / -nogroup-print
```

## 按照更改时间或访问时间等查找文件

如果希望按照更改时间来查找文件，可以使用mtime,atime或ctime选项。如果系统突然没有可用空间了，很有可能某一个文件的长度在此期间增长迅速，这时就可以用mtime选项来查找这样的文件。  

用减号-来限定更改时间在距今n日以内的文件，而用加号+来限定更改时间在距今n日以前的文件
```
# 希望在系统根目录下查找更改时间在5日以内的文件
find / -mtime -5 -print

# 为了在/var/adm目录下查找更改时间在3日以前的文件
find /var/adm -mtime +3 -print
```

## 查找比某个文件新或旧的文件

```
# 如果希望查找更改时间比某个文件新但比另一个文件旧的所有文件，可以使用-newer选项。
newest_file_name ! oldest_file_name  
# 其中，！是逻辑非符号。  

# 查找更改时间比文件log2012.log新但比文件log2017.log旧的文件
find -newer log2012.log ! -newer log2017.log

# 查找更改时间在比log2012.log文件新的文件  
find . -newer log2012.log -print
```

## 使用type选项
```
# 在/etc目录下查找所有的目录  
find /etc -type d -print  

# 在当前目录下查找除目录以外的所有类型的文件  
find . ! -type d -print  

# 在/etc目录下查找所有的符号链接文件
find /etc -type l -print
```

## 使用size选项

可以按照文件长度来查找文件，这里所指的文件长度既可以用块（block）来计量，也可以用字节来计量。以字节计量文件长度的表达形式为N c；以块计量文件长度只用数字表示即可。  

在按照文件长度查找文件时，一般使用这种以字节表示的文件长度，在查看文件系统的大小，因为这时使用块来计量更容易转换。  
```
# 在当前目录下查找文件长度大于1M字节的文件  
find . -size +1000000c -print

# 在/home 目录下查找文件长度为100字节的文件:  
find /home/ -size 100c -print  

# 在当前目录下查找长度超过10块的文件（一块等于512字节） 
find . -size +10 -print
```

## 使用depth选项

在使用find命令时，可能希望先匹配所有的文件，再在子目录中查找。使用depth选项就可以使find命令这样做。

这样做的一个原因就是，当在使用find命令向磁带上备份文件系统时，希望首先备份所有的文件，其次再备份子目录中的文件。  
```
# find命令从文件系统的根目录开始，查找一个名为 a.txt 的文件。将首先匹配所有的文件然后再进入子目录中查找
find / -name "a.txt" -depth -print
```

## 使用mount选项
在当前的文件系统中查找文件（不进入其他文件系统），可以使用find命令的mount选项。

```
# 从当前目录开始查找位于本文件系统中文件名以 swift 结尾的文件  
find . -name "*.swift" -mount -print 
```

