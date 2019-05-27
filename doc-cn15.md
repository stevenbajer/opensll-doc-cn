openssl简介－指令pkcs7

  用法: 
  
    openssl pkcs7 [-inform PEM|DER] [-outform PEM|DER] [-in filename] 

[-out filename] [-print_certs] [-text] [-noout] 


    说明: 
    处理PKCS#7文件的工具, 

OPTIONS 
    -inform DER|PEM 
    指定输入的格式是DEM还是DER. DER格式采用ASN1的DER标准格式。一般用的多的都是PEM格式，就是base64编码格式.你去看看你做出来的那些.key, .crt文件一般都是PEM格式的，第一行和最后一行指明内容，中间就是经过编码的东西。 
    -outform DER|PEM 
    和上一个差不多，不同的是指定输出格式 
    -in filename 
    要分析的文件名称, 缺省是标准输入. 
    -out filename 
    要输出的文件名, 缺省是标准输出. 
    write to or standard output by default. 
    -print_certs 
    打印出该文件内的任何证书或者CRL. 
    -text 
    打印出证书的细节. 
    -noout 
    不要打印出PKCS#7结构的编码版本信息. 
    举例时间: 
    把一个PKCS#7文件从PEM格式转换成DER格式 
    openssl pkcs7 -in file.pem -outform DER -out file.der 
    打印出文件内所有的证书 
    openssl pkcs7 -in file.pem -print_certs -out certs.pem 
    PCKS#7 文件的开始和结束俩行是这样子的: 
    -----BEGIN PKCS7----- 
    -----END PKCS7----- 
    为了和某些猥琐CA兼容,这样子的格式也可以接受 
    -----BEGIN CERTIFICATE----- 
    -----END CERTIFICATE----- 
    好象我们还没有解释pkcs#7是什么东西. 有兴趣的可以看看rfc2315, 估计看完目录还没有阵亡的同学不会超过1/10.

 

 