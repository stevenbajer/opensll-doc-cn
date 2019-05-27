openssl简介－指令x509

用法： 

openssl x509 [-inform DER|PEM|NET] [-outform DER|PEM|NET] 

[-keyform DER|PEM][-CAform DER|PEM] [-CAkeyform DER|PEM] 

[-in filename][-out filename] [-serial] [-hash] [-subject] 

[-issuer] [-nameopt option] [-email] [-startdate] [-enddate] 

[-purpose] [-dates] [-modulus] [-fingerprint] [-alias] 

[-noout] [-trustout] [-clrtrust] [-clrreject] [-addtrust arg] 

[-addreject arg] [-setalias arg] [-days arg] 

[-signkey filename][-x509toreq] [-req] [-CA filename] 

[-CAkey filename] [-CAcreateserial] [-CAserial filename] 

[-text] [-C] [-md2|-md5|-sha1|-mdc2] [-clrext] 

[-extfile filename] [-extensions section] 


    说明： 
    本指令是一个功能很丰富的证书处理工具。可以用来显示证书的内容，转换其格式，给CSR签名等等。由于功能太多，我们按功能分成几部分来讲。 
    输入，输出等一些一般性的option 
    -inform DER|PEM|NET 
    指定输入文件的格式。 
    -outform DER|PEM|NET 
    指定输出文件格式 
    -in filename 
    指定输入文件名 
    -out filename 
    指定输出文件名 
    -md2|-md5|-sha1|-mdc2 
    指定使用的哈希算法。缺省的是MD5于打印有关的option 
    -text 
    用文本方式详细打印出该证书的所有细节。 
    -noout 
    不打印出请求的编码版本信息。 
    -modulus 
    打印出公共密钥的系数值。没研究过RSA就别用这个了。 
    -serial 
    打印出证书的系列号。 
    -hash 
    把证书的拥有者名称的哈希值给打印出来。 
    -subject 
    打印出证书拥有者的名字。 
    -issuer 
    打印证书颁发者名字。 
    -nameopt option 
    指定用什么格式打印上俩个option的输出。 
    后面有详细的介绍。 
    -email 
    如果有，打印出证书申请者的email地址 
    -startdate 
    打印证书的起始有效时间 
    -enddate 
    打印证书的到期时间 
    -dates 
    把上俩个option都给打印出来 
    -fingerprint 
    打印DER格式的证书的DER版本信息。 
    -C 
    用C代码风格打印结果。 
    与证书信任有关的option 
    一个可以信任的证书的就是一个普通证书，但有一些附加项指定其可以用于哪些用途和不可以用于哪些用途, 该证书还应该有一个"别名"。 
    一般来说验证一个证书的合法性的时候，相关的证书链上至少有一个证书必须是一个可以信任的证书。缺省的认为如果该证书链上的Root CA的证书可以信任，那么整条链上其他证书都可以用于任何用途。 
    以下的几个option只用来验证Root CA的证书。CA在颁发证书的时候可以控制该证书的用途，比如颁发可以用于SSL client而不能用于SSL server的证书。 
    -trustout 
    打印出可以信任的证书。 
    -setalias arg 
    设置证书别名。比如你可以把一个证书叫"fordesign's certificate", 那么以后就可以使用这个别名来引用这个证书。 
    -alias 
    打印证书别名。 
   -clrtrust 
   清除证书附加项里所有有关用途允许的内容。 
   -clrreject 
   清除证书附加项里所有有关用途禁止的内容。 
   -addtrust arg 
   添加证书附加项里所有有关用途允许的内容。 
   -addreject arg 
   添加证书附加项里所有有关用途禁止的内容。 
   -purpose 
   打印出证书附加项里所有有关用途允许和用途禁止的内容。 

与签名有关的otpion 
    本指令可以用来处理CSR和给证书签名，就象一个CA 
    -signkey filename 
    使用这个option同时必须提供私有密钥文件。这样把输入的文件变成字签名的证书。 
    如果输入的文件是一个证书，那么它的颁发者会被set成其拥有者.其他相关的项也会被改成符合自签名特征的证书项。 
    如果输入的文件是CSR, 那么就生成自签名文件。 
    -clrext 
    把证书的扩展项删除。 
    -keyform PEM|DER 
    指定使用的私有密钥文件格式。 
    -days arg 
    指定证书的有效时间长短。缺省为30天。 
    -x509toreq 
    把一个证书转化成CSR.用-signkey指定私有密钥文件 
    -req 
    缺省的认为输入文件是证书文件，set了这个option说明输入文件是CSR. 
    -CA filename 
    指定签名用的CA的证书文件名。 
    -CAkey filename 
    指定CA私有密钥文件。如果这个option没有参数输入，那么缺省认为私有密钥在CA证书文件里有。 
    -CAserial filename 
    指定CA的证书系列号文件。证书系列号文件在前面介绍过，这里不重复了。 
    -CAcreateserial filename 
    如果没有CA系列号文件，那么本option将生成一个。 
    -extfile filename 
    指定包含证书扩展项的文件名。如果没有，那么生成的证书将没有任何扩展项。 
    -extensions section 
   指定文件中包含要增加的扩展项的section 
    上面俩个option有点难明白是吧？后面有举例。 
    与名字有关的option.这些option决定证书拥有者/颁发者的打印方式。缺省方式是印在一行中。 
    这里有必要解释一下这个证书拥有者/颁发者是什么回事。它不是我们常识里的一个名字，而是一个结构，包含很多字段。 
    英文分别叫subject name/issuer name.下面是一个subject name的例子 
    subject= 
    countryName = AU 
    stateOrProvinceName = Some-State 
    localityName = gz 
    organizationName = ai ltd 
    organizationalUnitName = sw 
    commonName = fordesign 
    emailAddress = xxx@xxx.xom


-nameopt 
    这个option后面的参数就是决定打印的方式，其参数有以下可选： 
    compat 
    使用以前版本的格式，等于没有设置任何以下option 
    RFC2253 
    使用RFC2253规定的格式。 
    oneline 
    所有名字打印在一行里面。 
   multiline 
    名字里的各个字段用多行打印出来。 
    上面那几个是最常用的了，下面的这些我怎么用怎么不对，也许以后研究source在完善这里了。 
    esc_2253 esc_ctrl esc_msb use_quote utf8 no_type show_type dump_der 
    dump_nostr dump_all dump_unknown sep_comma_plus sep_comma_plus_space 
    sep_semi_plus_space sep_multiline dn_rev nofname, sname, lname, oid spc_eq 
    举例时间： 
    打印出证书的内容： 
    openssl x509 -in cert.pem -noout -text 
    打印出证书的系列号 
    openssl x509 -in cert.pem -noout -serial 
    打印出证书的拥有者名字 
    openssl x509 -in cert.pem -noout -subject 
    以RFC2253规定的格式打印出证书的拥有者名字 
    openssl x509 -in cert.pem -noout -subject -nameopt RFC2253 
    在支持UTF8的终端一行过打印出证书的拥有者名字 
    openssl x509 -in cert.pem -noout -subject -nameopt oneline -nameopt -escmsb 
    打印出证书的MD5特征参数 
    openssl x509 -in cert.pem -noout -fingerprint 
    打印出证书的SHA特征参数 
    openssl x509 -sha1 -in cert.pem -noout -fingerprint 
    把PEM格式的证书转化成DER格式 
    openssl x509 -in cert.pem -inform PEM -out cert.der -outform DER 
    把一个证书转化成CSR 
    openssl x509 -x509toreq -in cert.pem -out req.pem -signkey key.pem 
    给一个CSR进行处理，颁发字签名证书，增加CA扩展项 
    openssl x509 -req -in careq.pem -extfile openssl.cnf -extensions v3_ca -signkey key.pem -out cacert.pem 
    给一个CSR签名，增加用户证书扩展项 
    openssl x509 -req -in req.pem -extfile openssl.cnf -extensions v3_usr -CA cacert.pem -CAkey key.pem -CAcreateserial 
    把某证书转化成用于SSL client可信任证书, 增加别名alias 
    openssl x509 -in cert.pem -addtrust sslclient -alias "Steve's Class 1 CA" -out trust.pem 
    上面有很多地方涉及到证书扩展/证书用途，这里解释一下： 
   我们知道一个证书是包含很多内容的，除了基本的那几个之外，还有很多扩展的项。比如证书用途，其实也只是证书扩展项中的一个。 
    我们来看看一个CA证书的内容： 
    openssl x509 -in ca.crt -noout -text 
    Certificate: 
    Data: 
    Version: 3 (0x2) 
    Serial Number: 0 (0x0) 
    Signature Algorithm: md5WithRSAEncryption 
    Issuer: C=AU, ST=Some-State, O=Internet Widgits Pty Ltd, 
    CN=fordesign/Email=fordeisgn@21cn.com 
    Validity 
    Not Before: Nov 9 04:02:07 2000 GMT 
    Not After : Nov 9 04:02:07 2001 GMT 
    Subject: C=AU, ST=Some-State, O=Internet Widgits Pty Ltd, 
    CN=fordesign/Email=fordeisgn@21cn.com 
    Subject Public Key Info: 
    Public Key Algorithm: rsaEncryption 
    RSA Public Key: (1024 bit) 
    Modulus (1024 bit): 
    00:e7:62:1b:fb:78:33:d7:fa:c4:83:fb:2c:65:c1: 
    08:03:1f:3b:79:b9:66:bb:31:aa:77:d4:47:ac:be: 
    e5:20:ce:ed:1f:b2:b5:4c:79:c9:9b:ad:1d:0b:7f: 
    84:49:03:6b:79:1a:fd:05:ca:36:b3:90:b8:5c:c0: 
    26:93:c0:02:eb:78:d6:8b:e1:91:df:85:39:33:fc: 
    3d:59:e9:7f:58:34:bf:be:ef:bd:22:a5:be:26:c0: 
    16:9b:41:36:45:05:fe:f9:b2:05:42:04:c9:3b:28: 
    c1:0a:48:f4:c7:d6:a8:8c:f9:2c:c1:1e:f5:8b:dc: 
    19:59:7c:47:f7:91:cc:5d:75 
    Exponent: 65537 (0x10001) 
    X509v3 extensions: 
    X509v3 Subject Key Identifier: 
    69:41:87:55:BD:52:99:D0:F5:EC:11:7F:0A:01:53:58:4E:0B:7C:F7 
    X509v3 Authority Key Identifier: 
    keyid:69:41:87:55:BD:52:99:D0:F5:EC:11:7F:0A:01:53:58: 
    4E:0B:7C:F7 
    DirName:/C=AU/ST=Some-State/O=Internet Widgits Pty 
    Ltd/CN=fordesign/Email=fordeisgn@21cn.com 
    serial:00 
    X509v3 Basic Constraints: 
    CA:TRUE 
    Signature Algorithm: md5WithRSAEncryption 
    79:14:99:4a:8f:64:63:ab:fb:ad:fe:bc:ba:df:53:97:c6:92: 
    41:4d:de:fc:59:98:39:36:36:8e:c6:05:8d:0a:bc:49:d6:20: 
    02:9d:a2:5f:0f:03:12:1b:f2:af:23:90:7f:b1:6a:86:e8:3e: 
    0b:2c:fd:11:89:86:c3:21:3c:25:e2:9c:de:64:7a:14:82:32: 
    22:e1:35:be:39:90:f5:41:60:1a:77:2e:9f:d9:50:f4:81:a4: 
    67:b5:3e:12:e5:06:da:1f:d9:e3:93:2d:fe:a1:2f:a9:f3:25: 
    05:03:00:24:00:f1:5d:1f:d7:77:8b:c8:db:62:82:32:66:fd: 
    10:fa 
    是否看到我们先提到过的subject name/issuer name.本证书中这俩个字段是一样的，明显是自签名证书，是一个Root CA的证书。从X509v3 extension开始就是证书扩展项了。 
    这里有个X509v3 Basic constraints. 里面的CA字段决定该证书是否可以做CA的证书，这里是TURE。如果这个字段没有，那么会根据其他内容决定该证书是否可以做CA证书。 
    如果是X509v1证书，又没有这个扩展项，那么只要subject name和issuer name相同，就认为是Root CA证书了。 
    本例的证书没有证书用途扩展项，它是一个叫keyUseage的字段。 
    举个例子就可以看出该字段目前可以有以下值 
    openssl x509 -purpose -in server.crt 
    Certificate purposes: 
    SSL client : Yes 
    SSL client CA : No 
    SSL server : Yes 
    SSL server CA : No 
    Netscape SSL server : Yes 
    Netscape SSL server CA : No 
    S/MIME signing : Yes 
     S/MIME signing CA : No 
    S/MIME encryption : Yes 
     S/MIME encryption CA : No 
     CRL signing : Yes 
    CRL signing CA : No 
    Any Purpose : Yes 
    Any Purpose CA : Yes 
    SSL Client 
    SSL Client CA 

每个值的具体意义应该可以看名字就知道了吧？ 
    X509指令在转化证书成CSR的时候没有办法把证书里的扩展项转化过去给CSR签名做证书的时候，如果使用了多个option,应该自己保证这些option没有冲突。
