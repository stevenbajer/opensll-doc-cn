openssl简介－指令enc

用法： 

openssl enc -ciphername [-in filename] [-out filename] [-pass arg] [-e] 

[-d] [-a] [-k password] [-kfile filename] [-K key] [-iv IV] [-p] 

[-P] [-bufsize number] [-debug] 


    说明： 
    对称加密算法工具。它能够把数据用不同对称加密算法来加/解密。还能够把加密/接密,还可以把结果进行base64编码。 

OPTIONS 
    -in filename 
    要加密/解密的输入文件，缺省为标准输入。 
    -out filename 
    要加密/解密的输出文件，缺省为标准输出。 
    -pass arg 
    输入文件如果有密码保护，在这里输入密码。 
    -salt 
    为了和openssl0.9.5以后的版本兼容，必须set这个option.salt大概又是密码学里的一个术语，具体是做什么的我也没弄的很明白。就我的理解,这是加密过后放在密码最前面的一段字符串, 用途也是为了让破解更难.如果理解错了,请密码学高手指正. 
    -nosalt 
    想和openssl0.9.5以前的版本兼容，就set这个option 
    -e 
    一个缺省会set的option, 把输入数据加密。 
    -d 
    解密输入数据。 
    -a 
    用base64编码处理数据。set了这个option表示在加密之后的数据还要用base64编码捏一次，解密之前则先用base64编码解码。 
    -k password 
    一个过时了的项，为了和以前版本兼容。现在用-key代替了。 
    -kfile filename 
    同上，被passin代替。 
    -K key 
    以16进制表示的密码。 
    -iv IV 
   作用完全同上。 
    -p 
    打印出使用的密码。 
    -P 
    作用同上，但打印完之后马上退出。 
    -bufsize number 
    设置I/O操作的缓冲区大小 
    -debug 
    打印调试信息。 

注意事项： 
    0.9.5以后的版本，使用这个指令，-salt是必须被set的。否则很容易用字典攻击法破你的密码，流加密算法也容易被破。(加密算法中有块加密算法和流加密算法俩种，块加密算法是一次加密固定长度的数据，一般是8Bytes, 流加密算法则加密大量数据)。为什么我也弄不清楚。研究加密算法实在麻烦，也不是我们程序员的责任本指令可以用不同加密算法，那么哪些好，哪些坏呢？如果你使用不当，高强度的加密算法也变脆弱了。一般推荐新手门使用des3-cbc。 

本指令支持的加密算法 
    base64 Base 64 
    bf-cbc Blowfish in CBC mode 

bf Alias for bf-cbc 

bf-cfb Blowfish in CFB mode 

bf-ecb Blowfish in ECB mode 

bf-ofb Blowfish in OFB mode 

cast-cbc CAST in CBC mode 

cast Alias for cast-cbc 

cast5-cbc CAST5 in CBC mode 

cast5-cfb CAST5 in CFB mode 

cast5-ecb CAST5 in ECB mode 

cast5-ofb CAST5 in OFB mode 
    des-cbc DES in CBC mode 

des Alias for des-cbc 

des-cfb DES in CBC mode 

des-ofb DES in OFB mode 

des-ecb DES in ECB mode 


     des-ede-cbc Two key triple DES EDE in CBC mode 

des-ede Alias for des-ede 

des-ede-cfb Two key triple DES EDE in CFB mode 

des-ede-ofb Two key triple DES EDE in OFB mode 

des-ede3-cbc Three key triple DES EDE in CBC mode 

des-ede3 Alias for des-ede3-cbc 

des3 Alias for des-ede3-cbc 

des-ede3-cfb Three key triple DES EDE CFB mode 

des-ede3-ofb Three key triple DES EDE in OFB mode 

desx DESX algorithm. 

idea-cbc IDEA algorithm in CBC mode 

idea same as idea-cbc 

idea-cfb IDEA in CFB mode 

idea-ecb IDEA in ECB mode 

idea-ofb IDEA in OFB mode 

rc2-cbc 128 bit RC2 in CBC mode 

rc2 Alias for rc2-cbc 

rc2-cfb 128 bit RC2 in CBC mode 

rc2-ecb 128 bit RC2 in CBC mode 

rc2-ofb 128 bit RC2 in CBC mode 

rc2-64-cbc 64 bit RC2 in CBC mode 

rc2-40-cbc 40 bit RC2 in CBC mode 


     rc4 128 bit RC4 

rc4-64 64 bit RC4 

rc4-40 40 bit RC4 


     rc5-cbc RC5 cipher in CBC mode 

rc5 Alias for rc5-cbc 

rc5-cfb RC5 cipher in CBC mode 

rc5-ecb RC5 cipher in CBC mode 

rc5-ofb RC5 cipher in CBC mode 

大家可能看到DES都分des-ecb, des-cbc, des-cfb这些。简单解释一下。 
    ecb就是说每来8bytes,就加密8bytes送出去。各个不同的数据块之间没有任何联系。cbc和cfb则每次加密一个8bytes的时候都和上一个8bytes加密的结果有一个运算法则。各个数据块之间是有联系的。 
    举例时间： 
    把某二进制文件转换成base64编码方式： 
    openssl base64 -in file.bin -out file.b64 
    把某base64编码文件转换成二进制文件。 
    openssl base64 -d -in file.b64 -out file.bin 
    把某文件用DES-CBC方式加密。加密过程中会提示你输入保护密码。 

openssl des3 -salt -in file.txt -out file.des3 
    解密该文件， 密码通过-k来输入 
    openssl des3 -d -salt -in file.des3 -out file.txt -k mypassword 
    加密某文件，并且把加密结果进行base64编码。用bf+cbc算法加密 
    openssl bf -a -salt -in file.txt -out file.bf 
    先用base64解码某文件，再解密 
    openssl bf -d -salt -a -in file.bf -out file.txt



