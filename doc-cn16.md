openssl简介－指令rand

用法: 

openssl rand [-out file] [-rand file(s)] [-base64] num 


    描述: 
    用来产生伪随机字节. 随机数字产生器需要一个seed, 先已经说过了,在没有/dev/srandom系统下的解决方法是自己做一个~/.rnd文件.如果该程序能让随机数字产生器很满意的被seeded,程序写回一些怪怪的东西回该文件. 

OPTIONS 
   -out file 
   输出文件. 
   -rand file(s) 
   产生随机数字的时候用过seed的文件，可以把多个文件用冒号分开一起做seed. 
   -base64 
   对产生的东西进行base64编码 
   num 
  指明产生多少字节随机数.

 

