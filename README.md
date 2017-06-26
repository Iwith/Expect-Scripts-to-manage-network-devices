# Expect-Scripts-to-manage-network-devices
These are simple code to manage device of network.

———————————————————————————————————————————————

The frist script is designed to backup network devices automatically.


#!/usr/bin/expect             #解释器

set ip 192.168.70.2           #这个脚本只设置了台机器的IP地址
set timeout 1
set username cisco
set password cisco
set timeout 3            
match_max 1000                #这个也是必须的

spawn telnet $ip   	          #进行 Telnet 操作
expect ":"
send "$username\n"            #有个回车确认
expect "Password:"
send "$password\n"
expect "*>"
send "en\n"
expect "Password:"
send "$password\n"
expect "*#"
expect "*#"
send  "show run\n" 
send "\ \ \ \ \ \ \ \ \ \  "   #如果出现要输出空格的交互，那么在代码中设置： "\ "   要使用转义字符对空格进行转义。
log_file router.log.$ip.test   #将输出导入到 router.log.$ip.test中，这个文件会根据ip不同而保存的文件名不一样。

send "exit\n"                  #退出
expect eof                     #结束此次的备份
