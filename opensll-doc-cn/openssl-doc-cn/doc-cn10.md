openssl简介－指令dhparam

用法： 

openssl dhparam [-inform DER|PEM] [-outform DER|PEM] [-in filename] 

[-out filename] [-dsaparam] [-noout] [-text] [-C] [-2] [-5] 

[-rand file(s)] [numbits] 


    描述： 
    本指令用来维护DH的参数文件。 

OPTIONS: 
    -inform DER|PEM 
    指定输入的格式是DEM还是DER. DER格式采用ASN1的DER标准格式。一般用的多的都是PEM格式，就是base64编码格式.你去看看你做出来的那些.key, .crt文件一般都是PEM格式的，第一行和最后一行指明内容，中间就是经过编码的东西。 
    -outform DER|PEM 
    和上一个差不多，不同的是指定输出格式 
    -in filename 
    要分析的文件名称。 
    -out filename 
    要输出的文件名。 
    -dsaparam 
    如果本option被set, 那么无论输入还是输入都会当做DSA的参数。它们再被转化成DH的参数格式。这样子产生DH参数和DH key都会块很多。会使SSL握手的时间缩短。当然时间是以安全性做牺牲的，所以如果这样子最好每次使用不同的参数，以免给人K破你的key. 
     -2, -5 
    使用哪个版本的DH参数产生器。版本2是缺省的。如果这俩个option有一个被set, 那么将忽略输入文件。 

-rand file(s) 
    产生key的时候用过seed的文件，可以把多个文件用冒号分开一起做seed. 
    numbits 
    指明产生的参数的长度。必须是本指令的最后一个参数。如果没有指明，则产生512bit长的参数。 


    -noout 
    不打印参数编码的版本信息。 
    -text 
    将DH参数以可读方式打印出来。 
    -C 
    将参数转换成C代码方式。这样可以用get_dhnumbits()函数调用这些参数。 

openssl还有俩个指令， dh, gendh, 现在都过时了，全部功能由dhparam实现。 
    现在dh, gendh这俩个指令还保留，但在将来可能会用做其他用途。



