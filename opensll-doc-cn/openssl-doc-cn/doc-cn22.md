openssl简介－指令sess_id

用法： 

openssl sess_id [-inform PEM|DER] [-outform PEM|DER] [-in filename] 

[-out filename] [-text] [-noout] [-context ID] 


    说明： 
    本指令是处理SSL_SESSION结构的，可以打印出其中的细节。这也是一个调试工具。 
    -inform DER|PEM 
    指定输入格式是DER还是PEM. 
    -outform DER|PEM 
    指定输出格式是DER还是PEM 
   -in filename 
   指定输入的含有session信息的文件名，可以通过标准输入得到。 
   -out filename 
   指定输出session信息的文件名 
   -text 
   打印出明文的密钥的各个部件。 
   -cert 
   set本option将会把session中使用的证书打印出来。如果-text也被set, 那么将会把其用文本格式打印出来。 
    -noout 
    不打印出session的编码版本。 
    -context ID 
    设置session id. 不常用的一个option. 
    本指令的典型的输出是： 
    SSL-Session: 
    Protocol : TLSv1 
    Cipher : 0016 
    Session-ID: 871E62626C554CE95488823752CBD5F3673A3EF3DCE9 
    C67BD916C809914B40ED 
    Session-ID-ctx: 01000000 
    Master-Key: A7CEFC571974BE02CAC305269DC59F76EA9F0B180CB66 
    42697A68251F2D2BB57E51DBBB4C7885573192AE9AEE220FACD 
    Key-Arg : None 
    Start Time: 948459261 
   Timeout : 300 (sec) 
    Verify return code 0 (ok) 
    Protocol 
    使用的协议版本信息。 
    Cipher 
    使用的cipher, 这里是原始的SSL/TLS里定义的代码。 
    Session-ID 
    16进制的session id 
    Session-ID-ctx 
    session-id-ctx的16进制格式。 
    Master-Key 
    ssl session master key. 
    Key-Arg 
    key的参数，只用于SSLv2 
    Start Time 
    session开始的时间。标准的unix格式。 
    Timeout 
    session-timeout时间。 
    Verify return code 
    证书验证返回值. 
    ssl session文件的pem标准格式的第一行和最后一行是： 
    ---BEGIN SSL SESSION PARAMETERS----- 
    -----END SSL SESSION PARAMETERS----- 
    因为ssl session输出包含握手的重要信息：master key, 所以一定要用一定的加密算法把起输出加密。一般是禁止在实际应用中把session的信息输出。我没用过这个工具。研究source的时候这个可能有点用。


