openssl简介－指令gendsa

用法： 

openssl gendsa [-out filename] [-des] [-des3] [-idea] 

[-rand file(s)] [paramfile] 

描述： 

本指令由DSA参数来产生DSA的一对密钥。dsa参数可以用dsaparam来产生。 

OPTIONS 
    -des|-des3|-idea 
    采用什么加密算法来加密我们的密钥。一般会要你输入保护密码。 
    如果这三个中一个也没set, 我们的密钥将不被加密而输入。 
    -rand file(s) 
    产生key的时候用过seed的文件，可以把多个文件用冒号分开一起做seed. 
    paramfile 
    指定使用的DSA参数文件。

 

