openssl简介－指令genrsa

用法： 

openssl genrsa [-out filename] [-passout arg] [-des] [-des3] [-idea] 

[-f4] [-3] [-rand file(s)] [numbits] 


    DESCRIPTION 
    生成RSA私有密钥的工具。 


    OPTIONS 
    -out filename 
    私有密钥输入文件名，缺省为标准输出。 
    the output filename. If this argument is not specified then standard output is uused. 
    -passout arg 
    参看指令dsa里面的passout参数说明 
    -des|-des3|-idea 
    采用什么加密算法来加密我们的密钥。一般会要你输入保护密码。 
    如果这三个中一个也没set, 我们的密钥将不被加密而输入。 
    -F4|-3 
    使用的公共组件，一种是3, 一种是F4, 我也没弄懂这个option是什么意思。 
    -rand file(s) 
    产生key的时候用过seed的文件，可以把多个文件用冒号分开一起做seed. 
    numbits 
    指明产生的参数的长度。必须是本指令的最后一个参数。如果没有指明，则产生512bit长的参数。 
    研究过RSA算法的人肯定知道，RSA的私有密钥其实就是三个数字，其中俩个是质数。这俩个呢，就叫prime numbers.产生RSA私有密钥的关键就是产生这俩。还有一些其他的参数，引导着整个私有密钥产生的过程。因为产生私有密钥过程需要很多随机数，这个过程的时间是不固定的。 
    产生prime numbers的算法有个bug, 它不能产生短的primes. key的bits起码要有64位。一般我们都用1024bit的key.



