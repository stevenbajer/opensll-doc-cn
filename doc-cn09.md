openssl简介－指令dgst

用法： 

openssl dgst [md5|md2|sha1|sha|mdc2|ripemd160] [-c] [-d] [file...] 

说明：这个指令可以用来哈希某个文件内容的，以前的版本还可以用来做数字签名和认证。这个工具本来有很多选项的，可是不知道为什么，现在版本的openssl删掉了很多。表示你用什么算法来哈希该文件内容 

OPTIONS 
    -md5 -sha那些就不用结实了吧，都是一些哈希算法的名称 
    -c 
    打印出哈希结果的时候用冒号来分隔开。 
    -d 
    详细打印出调试信息 
    file... 
    你要哈希的文件，如果没有指定，就使用标准输入。 
    举例时间： 
    要哈希一个叫fordesign.txt文件的内容，使用SHA算法 
    openssl dgst -sha -c fordesign.txt 
    SHA(fordesign.txt)= 
    57:37:dc:a5:8c:bd:12:aa:43:45:fe:2a:19:f5:05:a3:be:e9:08:cc



