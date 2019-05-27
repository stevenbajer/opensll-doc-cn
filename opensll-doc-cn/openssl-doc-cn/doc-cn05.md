openssl简介－指令 verify
用法： 

openssl verify 【-CApath directory】 【-CAfile file】 【-purpose purpose】【-untrusted file】 【-help】 【-issuer_checks】 【-verbose】  【-】 【certificates】 

说明： 

证书验证工具。 

选项 
     -CApath directory 
    我们信任的CA的证书存放目录。这些证书的名称应该是这样的格式： 
    xxxxxxxx.0( xxxxxxxx代表证书的哈希值。 参看x509指令的-hash) 
    你也可以在目录里touch一些这样格式文件名的文件，符号连接到真正的证书。 
    那么这个xxxxxxxx我怎么知道怎么得到？x509指令有说明。 
    其实这样子就可以了： 
    openssl x509 -hash -in server.crt 

-CAfile file 
    我们信任的CA的证书，里面可以有多个CA的证书。 

-untrusted file 
    我们不信任的CA的证书。 

-purpose purpose 
    证书的用途。如果这个option没有设置，那么不会对证书的CA链进行验证。 

现在这个option的参数有以下几个： 
    sslclinet 
    sslserver 
    nssslserver 
    smimesign 
    smimeencrypt 
    等下会详细解释的。 

-help 
    打印帮助信息。 

-verbose 

打印出详细的操作信息。 

-issuer_checks 
    打印出我们验证的证书的签发CA的证书的之间的联系。 
    要一次验证多个证书，把那些证书名都写在后面就好了。 

验证操作解释： 
    S/MIME和本指令使用完全相同的函数进行验证。 
    我们进行的验证和真正的验证有个根本的区别： 
    在我们对整个证书链进行验证的时候，即使中途有问题，我们也会验证到最后，而真实的验证一旦有一个环节出问题，那么整个验证过程就告吹。 
    验证操作包括几个独立的步骤。 
    首先建立证书链，从我们目前的证书为基础，一直上溯到Root CA的证书. 
    如果中间有任何问题，比如找不到某个证书的颁发者的证书，那么这个步骤就挂。有任何一个证书是字签名的，就被认为是Root CA的证书。 
    寻找一个证书的颁发CA也包过几个步骤。在openssl0.9.5a之前的版本，如果一个证书的颁发者和另一个证书的拥有着相同，就认为后一个证书的拥有者就是前一个证书的签名CA. 
    openssl0.9.6及其以后的版本中，即使上一个条件成立，还要进行更多步骤的检验。包括验证系列号等。到底有哪几个我也没看明白。 
    得到CA的名称之后首先去看看是否是不信任的CA, 如果不是，那么才去看看是否是信任的CA. 尤其是Root CA, 更是必须是在信任CA列表里面。 
    现在得到链条上所有CA的名称和证书了，下一步是去检查第一个证书的用途是否和签发时候批准的一样。其他的证书则必须都是作为CA证书而颁发的。 
    证书的用途在x509指令里会详细解释。 
    过了第二步，现在就是检查对Root CA的信任了。可能Root CA也是每个都负责不同领域的证书签发。缺省的认为任何一个Root CA都是对任何用途的证书有签发权。 
    最后一步，检查整条证书链的合法性。比如是否有任何一个证书过期了？签名是否是正确的？是否真的是由该证书的颁发者签名的？ 
    任何一步出问题，所有该证书值得怀疑，否则，证书检验通过。 

如果验证操作有问题了，那么打印出来的结果可能会让人有点模糊。 
    一般如果出问题的话，会有类似这样子的结果打印出来： 
    server.pem: /C=AU/ST=Queensland/O=CryptSoft Pty Ltd/CN=Test CA (1024 bit) 
    error 24 at 1 depth lookup:invalid CA certificate 
    第一行说明哪个证书出问题，后面是其拥有者的名字，包括几个字段。 
    第二行说明错误号，验证出错在第几层的证书，以及错误描述。 
    下面是错误号及其描述的详细说明,注意，有的错误虽然有定义， 
    但真正使用的时候永远不会出现。用unused标志. 
    0 X509_V_OK 
    验证操作没有问题 
    2 X509_V_ERR_UNABLE_TO_GET_ISSUER_CERT 
    找不到该证书的颁发CA的证书。 
    3 X509_V_ERR_UNABLE_TO_GET_CRL (unused) 
   找不到和该证书相关的CRL 
   4 X509_V_ERR_UNABLE_TO_DECRYPT_CERT_SIGNATURE 
   无法解开证书里的签名。 
    5 X509_V_ERR_UNABLE_TO_DECRYPT_CRL_SIGNATURE (unused) 
    无法解开CRLs的签名。 
    6 X509_V_ERR_UNABLE_TO_DECODE_ISSUER_PUBLIC_KEY 
    无法得到证书里的公共密钥信息。 
    7 X509_V_ERR_CERT_SIGNATURE_FAILURE 
    证书签名无效 
    8 X509_V_ERR_CRL_SIGNATURE_FAILURE (unused) 
    证书相关的CRL签名无效 
    9 X509_V_ERR_CERT_NOT_YET_VALID 
    证书还没有到有效开始时间 
    10 X509_V_ERR_CRL_NOT_YET_VALID (unused) 
    与证书相关的CRL还没有到有效开始时间 
    11 X509_V_ERR_CERT_HAS_EXPIRED 
    证书过期 
    12 X509_V_ERR_CRL_HAS_EXPIRED (unused) 
    与证书相关的CRL过期 
    13 X509_V_ERR_ERROR_IN_CERT_NOT_BEFORE_FIELD 
    证书的notBefore字段格式不对,就是说那个时间是非法格式。 
    14 X509_V_ERR_ERROR_IN_CERT_NOT_AFTER_FIELD 
    证书的notAfter字段格式不对,就是说那个时间是非法格式。 
    15 X509_V_ERR_ERROR_IN_CRL_LAST_UPDATE_FIELD (unused) 
    CRL的lastUpdate字段格式不对。 
    16 X509_V_ERR_ERROR_IN_CRL_NEXT_UPDATE_FIELD (unused) 
    CRL的nextUpdate字段格式不对 
    17 X509_V_ERR_OUT_OF_MEM 
    操作时候内存不够。这和证书本身没有关系。 
    18 X509_V_ERR_DEPTH_ZERO_SELF_SIGNED_CERT 
    需要验证的第一个证书就是字签名证书，而且不在信任CA证书列表中。 
    19 X509_V_ERR_SELF_SIGNED_CERT_IN_CHAIN 
    可以建立证书链，但在本地找不到他们的根？？ 

: self signed certificate in certificate chain 
    the certificate chain could be built up using the untrusted certificates 
    but the root could not be found locally. 
    20 X509_V_ERR_UNABLE_TO_GET_ISSUER_CERT_LOCALLY 
    有一个证书的签发CA的证书找不到。这说明可能是你的Root CA的证书列表不齐全。 
    21 X509_V_ERR_UNABLE_TO_VERIFY_LEAF_SIGNATURE 
    证书链只有一个item, 但又不是字签名的证书。 
    22 X509_V_ERR_CERT_CHAIN_TOO_LONG (unused) 
    证书链太长。 
    23 X509_V_ERR_CERT_REVOKED (unused) 
    证书已经被CA宣布收回。 
    24 X509_V_ERR_INVALID_CA 
    某CA的证书无效。 
    25 X509_V_ERR_PATH_LENGTH_EXCEEDED 
    参数basicConstraints pathlentgh超过规定长度 
    26 X509_V_ERR_INVALID_PURPOSE 
    提供的证书不能用于请求的用途。 
    比如链条中某个证书应该是用来做CA证书的，但证书里面的该字段说明该证书不是用做CA证书的，就是这样子的情况。 
    27 X509_V_ERR_CERT_UNTRUSTED 
    Root CA的证书如果用在请求的用途是不被信任的。 
    28 X509_V_ERR_CERT_REJECTED 
    CA的证书根本不可以用做请求的用途。 
    29 X509_V_ERR_SUBJECT_ISSUER_MISMATCH 
    证书颁发者名称和其CA拥有者名称不相同。-issuer_checks被set的时候可以检验出来。 
    30 X509_V_ERR_AKID_SKID_MISMATCH 
    证书的密钥标志和其颁发CA为其指定的密钥标志不同. 
    31 X509_V_ERR_AKID_ISSUER_SERIAL_MISMATCH 
    证书系列号与起颁发CA为其指定的系列号不同。 
    32 X509_V_ERR_KEYUSAGE_NO_CERTSIGN 
    某CA的证书用途不包括为其他证书签名。 
    50 X509_V_ERR_APPLICATION_VERIFICATION 
    应用程序验证出错。



