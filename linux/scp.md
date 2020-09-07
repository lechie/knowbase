# scp

scp 是 secure copy 的简写，是linux系统下基于ssh登陆进行安全的远程文件拷贝命令。
scp 命令与 cp 命令类似，不过cp只是在本机进行拷贝不能跨服务器，而且scp传输是加密的。
另外，scp还非常不占资源，不会提高多少系统负荷，在这一点上，rsync就远远不及它了。虽然 rsync比scp会快一点，但当小文件众多的情况下，rsync会导致硬盘I/O非常高，而scp基本不影响系统正常使用。

## 语法
scp [参数] [原路径] [目标路径]


## 参数
参数|说明
--|--
-1 |强制scp命令使用协议ssh1  
-2  |强制scp命令使用协议ssh2  
-4  |强制scp命令只使用IPv4寻址  
-6 | 强制scp命令只使用IPv6寻址  
-B  |使用批处理模式（传输过程中不询问传输口令或短语）  
-C  |允许压缩（将-C标志传递给ssh，从而打开压缩功能）  
-p |保留原文件的修改时间，访问时间和访问权限
-q  |不显示传输进度条
**-r**  |**递归复制整个目录**
-v |详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题
-c cipher  |以cipher将数据传输进行加密，这个选项将直接传递给ssh
-F ssh_config  |指定一个替代的ssh配置文件，此参数直接传递给ssh
-i identity_file  |从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh
-l limit  |限定用户所能使用的带宽，以Kbit/s为单位
-o ssh_option  |如果习惯于使用ssh_config(5)中的参数传递方式
-P port  |注意是大写的P, port是指定数据传输用到的端口号 
-S program  |指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项

## 示例

```
# 从本地复制文件到远程
scp local_file remote_username@remote_ip:remote_folder
scp local_file remote_username@remote_ip:remote_file  
scp local_file remote_ip:remote_folder  
scp local_file remote_ip:remote_file  

> 指定用户名，命令执行后需输入密码，否则命令执行后需要输入用户名和密码
> 仅指定远程的目录，文件名字不变，否则使用新指定的文件名


# 从本地复制目录到远程
scp -r local_folder remote_username@remote_ip:remote_folder 
scp -r local_folder remote_ip:remote_folder  

# 从远程服务器复制到本地服务器 
scp remote_username@remote_ip:remote_folder local_file 
...
> 命令与上面的命令类似，只要将2个参数互换顺序即可
```

```
# 从远程复制文件到本地目录
scp root@192.168.1.102:/home/test/a.tar.gz /data/test/


# 从远处复制到本地
scp -r root@192.168.1.102:/home/test/ /data/test/


# 上传本地文件到远程机器指定目录
scp /data/test/a.tar.gz root@192.168.1.102:/home/test/


# 上传本地目录到远程机器指定目录
scp -r /data/test/ root@192.168.1.102:/home/test/
```
