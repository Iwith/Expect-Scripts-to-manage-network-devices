# Expect-Scripts-to-manage-network-devices
These are simple code to manage device of network.

———————————————————————————————————————————————

The frist script is designed to backup network devices automatically.


    #!/usr/bin/expect             #解释器

    set ip 192.168.70.2           #这个脚本只设置了台机器的IP地址

    set timeout 1

    set username cisco

    set password cisco

    set timeout 3            
    match_max 1000                 #这个也是必须的

    spawn telnet $ip               #进行 Telnet 操作

    expect ":"

    send "$username\n"             #有个回车确认

    expect "Password:"

    send "$password\n"

    expect "*>"

    send "en\n"

    expect "Password:"

    send "$password\n"

    expect "*#"

    expect "*#"

    send  "show run\n" 

    send "\ \ \ \ \ \ \ \ \ \  "    #如果出现要输出空格的交互，那么在代码中设置： "\ "   要使用转义字符对空格进行转义。

    log_file router.log.$ip.test    #将输出导入到 router.log.$ip.test中，这个文件会根据ip不同而保存的文件名不一样。

    send "exit\n"                   #退出

    expect eof                      #结束此次的备份 

———————————————————————————————————————————————

↓This is the second scripts:     

#the file name "telnet.test"#

    #!/usr/bin/expect       
    set ip [lindex $argv 0]                    #设置 IP 变量
    set timeout 1       
    set username cisco      
    set password cisco      
    set timeout 3            
    match_max 1000            
    spawn telnet $ip   	  
    expect ":"
    send "$username\n"
    expect "Password:"
    send "$password\n"
    expect "*>"
    send "en\n"
    expect "Password:"
    send "$password\n"
    expect "*#"
    send "terminal length 0\n"                  #对交换机输出结果一次性显示完！        
    expect "*#"     
    send  "show run\n"      
    log_file router.log.$ip.test                #保存的配置的文件名，目录如果没有配置的话，默认是跟脚本在同一个文件夹中。       
    send "terminal length 48\n"                 #对交换机的输出一次性只显示48行！      
    send "exit\n"       
    expect eof      
    
    
↓这是一个循环，对多台设备进行备份：

#the file name "loop.sh"#

    #!/bin/bash     
    while read ip                               #循环执行 telnet.test 这个脚本      
    do      
    ./telnet.test $ip       
    done < ip.txt                               #将ip.txt中的ip地址作为变量进行导入      


↓这是要备份的设备的列表：  

#file name "ip.txt"#  

    192.168.1.1     
    192.168.1.2     
    192.168.1.3     
    ...     
