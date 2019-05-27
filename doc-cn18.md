openssl简介－指令rsa

    用法 

openssl rsa [-inform PEM|NET|DER] [-outform PEM|NET|DER] [-in filename] 

[-passin arg] [-out filename] [-passout arg] [-sgckey] [-des] [-des3] 

[-idea] [-text] [-noout] [-modulus] [-check] [-pubin] [-pubout] 


    说明: 
   rsa指令专门处理RSA密钥.其实其用法和dsa的差不多. 

OPTIONS 
    -inform DER|PEM|NET 
    指定输入的格式是DEM还是DER还是NET.注意, 这里多了一种格式,就是NET,DER格式采用ASN1的DER标准格式。一般用的多的都是PEM格式，就是base64编码格式.你去看看你做出来的那些.key, .crt文件一般都是PEM格式的，第一行和最后一行指明内容，中间就是经过编码的东西。NET格式在本章后面会详细解释. 
    -outform DER|PEM|NET 
    和上一个差不多，不同的是指定输出格式 
    -in filename 
    要分析的文件名称。如果文件有密码保护,会要你输入的. 
    -passin arg 
    去看看CA那一章关于这个option的解释吧。 
    -out filename 
    要输出的文件名。 
    -passout arg 
    没什么用的一个选项，用来把保护key文件的密码输出的，意义和passin差不多。 
    -sgckey 
    配合NET格式的私有密钥文件的一个option, 没有必要去深入知道了。 
    -des|-des3|-idea 
    指明用什么加密算法把我们的私有密钥加密。加密的时候会需要我们输入密码来保护该文件的。如果这仨一个都没有选，那么你的私有密钥就以明文写进你的key文件。该选项只能输出PEM格式的文件。 
    -text 
    打印出私有密钥的各个组成部分. 
    -noout 
    不打印出key的编码版本信息。 
    -modulus 
    把其公共密钥的值也打印出来 
    -pubin 
    缺省的来说是从输入文件里读到私有密钥，这个就可以从输入文件里去读公共密钥. 
    -pubout 
    缺省的来说是打印出私有密钥，这个就可以打印公共密钥.如果上面那个选项有set那么这个选项也自动被set. 
    -check 
    检查RSA的私有密钥是否被破坏了这个指令实在和dsa太相似了。copy的我手软。 
    现在解释一下NET是一种什么格式。它是为了和老的netscape server以及IIS兼容才弄出来的。他使用没有被salt过的RC4做加密算法，加密强度很底，如果不是一定要用就别用。 
    举例时间： 
    把RSA私有密钥文件的保护密码去掉（最好别这么做） 
    openssl rsa -in key.pem -out keyout.pem 
    用DES3算法加密我们的私有密码文件： 
    openssl rsa -in key.pem -des3 -out keyout.pem 
    把一个私有密钥文件从PEM格式转化成DER格式： 
    openssl rsa -in key.pem -outform DER -out keyout.der 
    把私有密钥的所有内容详细的打印出来： 
    openssl rsa -in key.pem -text -noout 
    只打印出公共密钥部分： 
    openssl rsa -in key.pem -pubout -out pubkey.pem



