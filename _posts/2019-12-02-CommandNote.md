---
title: Command Line Note
tags: Note
---

1. linux 复制文件命令  
``` linux
cp [option...] source destination 
```

2. 登录Redis命令  
``` 
redis-cli -h $host -a $password
```

3. windows查看端口占用命令  
``` 
netstat -aon|findstr "port"
tasklist|findstr "pid"
taskkill /f /t /im "pid/pname"
```

4. linux查看端口命令
```
netstat -tunlp|grep $port
-t (tcp) 仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化为数字
-l 仅列出在Listen(监听)的服务状态
-p 显示建立相关链接的程序名
```

5. linxu 查找文件/文件夹
find / -name $file_name
