openssl简介－指令passwd

SYNOPSIS 

openssl passwd [-crypt] [-1] [-apr1] [-salt string] [-in file] [-stdin] 

[-quiet] [-table] {password} 

说明： 
    本指令计算用来哈希某个密码，也可以用来哈希文件内容。 
    本指令支持三种哈希算法： 
    UNIX系统的标准哈希算法(crypt) 
    MD5-based BSD(1) 

OPTIONS 
   -crypt -1 -apr1 
    这三个option中任意选择一个作为哈希算法，缺省的是-crypt 
    -salt string 
    输入作为salt的字符串。 
    -in file 
    要哈希的文件名称 
    -stdin 
    从标准输入读入密码 
    -quiet 
    当从标准输入读密码，输入的密码太长的时候，程序将自动解短它。这个option的 
    set将不在情况下发出警告。 
    -table 
    在输出列的时候,先输出明文的密码,然后输出一个TAB,再输出哈希值. 
    举例时间: 
    openssl passwd -crypt -salt xx password xxj31ZMTZzkVA. 
    openssl passwd -1 -salt xxxxxxxx password $1$xxxxxxxx$8XJIcl6ZXqBMCK0qFevqT1. 
    openssl passwd -apr1 -salt xxxxxxxx password $apr1$xxxxxxxx$dxHfLAsjHkDRmG83UXe8K0



