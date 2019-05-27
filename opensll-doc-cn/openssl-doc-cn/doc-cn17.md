openssl简介－指令req

用法: 

openssl req [-inform PEM|DER] [-outform PEM|DER] [-in filename] 

[-passin arg] [-out filename] [-passout arg] [-text] [-noout] 

[-verify] [-modulus] [-new] [-rand file(s)] [-newkey rsa] 

[-newkey dsa] [-nodes] [-key filename] [-keyform PEM|DER] 

[-keyout filename] [-[md5|sha1|md2|mdc2]] [-config filename] 

[-x509] [-days n] [-asn1-kludge] [-newhdr] [-extensions section] 

[-reqexts section] 

描述: 
    本指令用来创建和处理PKCS#10格式的证书.它还能够建立自签名证书,做Root CA. 

OPTIONS 
    -inform DER|PEM 
    指定输入的格式是DEM还是DER. DER格式采用ASN1的DER标准格式。一般用的多的都是PEM格式，就是base64编码格式.你去看看你做出来的那些.key, .crt文件一般都是PEM格式的，第一行和最后一行指明内容，中间就是经过编码的东西。 
    -outform DER|PEM 
    和上一个差不多，不同的是指定输出格式 
    -in filename 
    要处理的CSR的文件名称,只有-new和-newkey俩个option没有被set,本option才有效 
    -passin arg 
    去看看CA那一章关于这个option的解释吧。 
     -out filename 
    要输出的文件名 
    -passout arg 
    参看dsa指令里的passout这个option的解释吧. 
   -text 
   将CSR文件里的内容以可读方式打印出来 
    -noout 
    不要打印CSR文件的编码版本信息. 
    -modulus 
    将CSR里面的包含的公共米要的系数打印出来. 
    -verify 
    检验请求文件里的签名信息. 
    -new 
    本option产生一个新的CSR, 它会要用户输入创建CSR的一些必须的信息.至于需要哪些信息,是在config文件里面定义好了的.如果-key没有被set, 那么就将根据config文件里的信息先产生一对新的RSA密钥 
    -rand file(s) 
    产生key的时候用过seed的文件，可以把多个文件用冒号分开一起做seed. 
    -newkey arg 
    同时生成新的私有密钥文件和CSR文件. 本option是带参数的.如果是产生RSA的私有密钥文件,参数是一个数字, 指明私有密钥bit的长度. 如果是产生DSA的私有密钥文件,参数是DSA密钥参数文件的文件名. 
    -key filename 
    参数filename指明我们的私有密钥文件名.允许该文件的格式是PKCS#8. 
    -keyform DER|PEM 
    指定输入的私有密钥文件的格式是DEM还是DER. DER格式采用ASN1的DER标准格式。一般用的多的都是PEM格式，就是base64编码格式.你去看看你做出来的那些.key, .crt文件一般都是PEM格式的，第一行和最后一行指明内容，中间就是经过编码的东西。 
    -outform DER|PEM 
    和上一个差不多，不同的是指定输出格式 
    -keyform PEM|DER 
    私有密钥文件的格式, 缺省是PEM 
    -keyout filename 
    指明创建的新的私有密钥文件的文件名. 如果该option没有被set, 将使用config文件里面指定的文件名. 
    -nodes 
    本option被set的话,生成的私有密钥文件将不会被加密. 
    -[md5|sha1|md2|mdc2] 
    指明签发的证书使用什么哈希算法.如果没有被set, 将使用config文件里的相应item的设置. 但DSA的CSR将忽略这个option, 而采用SHA1哈希算法. 
    -config filename 
    使用的config文件的名称. 本option如果没有set, 将使用缺省的config文件. 
    -x509 
    本option将产生自签名的证书. 一般用来错测试用,或者自己玩下做个Root CA.证书的扩展项在 config文件里面指定. 
   -days n 
   如果-509被set, 那么这个option的参数指定我们自己的CA给人家签证书的有效期.缺省是30天. 
    -extensions section -reqexts section 
    这俩个option指定config文件里面的与证书扩展和证书请求扩展有关的俩个section的名字(如果-x509这个option被set).这样你可以在config文件里弄几个不同的与证书扩展有关的section, 然后为了不同的目的给CSR签名的时候指明不同的section来控制签名的行为. 
    -asn1-kludge 
    缺省的req指令输出完全符合PKCS10格式的CSR, 但有的CA仅仅接受一种非正常格式的CSR, 这个option的set就可以输出那种格式的CSR. 要解释这俩种格式有点麻烦, 需要用到ASN1和PKCS的知识,而且现在这样子怪的CA几乎没有,所以省略解释 
    -newhdr 
    在CSR问的第一行和最后一行中加一个单词"NEW", 有的软件(netscape certificate server)和有的CA就有这样子的怪癖嗜好.如果那些必须要的option的参数没有在命令行给出，那么就会到config文件里去查看是否有缺省值， 然后时候。config文件中相关的一些KEY的解释与本指令有关的KEY都在[req]这个section里面. 
    input_password output_password 
    私有密钥文件的密码和把密码输出的文件名.同指令的passin, passout的意义相同. 
    default_bits 
    指定产生的私有密钥长度, 如果为空,那么就用512.只有-new被set, 这个设置才起作用,意义同-newkey相同. 
    default_keyfile 
    指定输出私有密钥文件名,如果为空, 将输出到标准输入,意义同-keyout相同. 
    oid_file 
    oid_section 
    与oid文件有关的项, oid不清楚是什么东西来的. 
    RANDFILE 
    产生随机数字的时候用过seed的文件，可以把多个文件用冒号分开一起做seed. 
    encrypt_key 
    如果本KEY设置为no, 那么如果生成一个私有密钥文件,将不被加密.同命令行的-nodes的意义相同. 
    default_md 
    指定签名的时候使用的哈希算法,缺省为MD5. 命令行里有同样的功能输入. 
    string_mask 
    屏蔽掉某些类型的字符格式. 不要乱改这个KEY的值!!有的字符格式netscape不支持,所以乱改这个KEY很危险. 
    req_extensions 
    指明证书请求扩展section, 然后由那个secion指明扩展的特性. openssl的缺省config文件里, 扩展的是X509v3, 不扩展的是x509v1.这个KEY的意义和命令行里-reqexts相同. 
    x509_extensions 
    同命令行的-extension的意义相同.指明证书扩展的sesion, 由那个section指明证书扩展的特性. 
    prompt 
    如果这个KEY设置为no, 那么在生成证书的时候需要的那些信息将从config文件里读入,而不是从标准输入由用户去输入, 同时改变下俩个KEY所指明的section的格式. 
    attributes 
    一个过时了的东西, 不知道也罢. 不过它的意义和下一个KEY有点类似, 
    格式则完全相同.

distinguished_name 
   指定一个section, 由那个section指定在生成证书或者CRS的时候需要的资料.该section的格式如下: 
    其格式有俩种, 如果KEY prompt被set成no(看看prompt的解释), 那么这个secion的格式看起来就是这样子的: 
     CN=My Name 
    OU=My Organization 
    emailAddress=someone@somewhere.org 
    就说只包括了字段和值。这样子可以可以让其他外部程序生成一个摸板文件,包含所有字段和值, 把这些值提出来.等下举例时间会有详细说明.如果prompt没有被set成no, 那么这个section的格式则如下: 
    fieldName="please input ur name" 

fieldName_default="fordesign" 

fieldName_min= 3 

fieldName_max= 18 

"fieldname"就是字段名, 比如commonName(或者CN). fieldName(本例中是"prompt")是用来提示用户输入相关的资料的.如果用户什么都不输, 那么就使用确省值.如果没有缺省值, 那么该字段被忽略.用户如果输入 '.' ,也可以让该字段被忽略. 
    用户输入的字节数必须在fieldName_min和fieldName_max之间. 不同的section还可能对输入的字符有特殊规定,比如必须是可打印字符.那么在本例里面, 程序的表现将如下: 
    首先把fieldName打印出来给用户以提示 
    please input ur name: 
    之后如果用户必须输入3到18之间的一个长度的字符串, 如果用户什么也不输入,那么就把fieldName_default里面的值"fordesign"作为该字段的值添入. 
    有的字段可以被重复使用.这就产生了一个问题, config文件是不允许同样的section文件里面有多于一个的相同的key的.其实这很容易解决,比如把它们的名字分别叫做 "1.organizationName", "2.organizationName" 
    openssl在编译的时候封装了最必须的几个字段， 比如commonName, countryName, localityName, organizationName,organizationUnitName, stateOrPrivinceName还增加了emailAddress surname, givenName initials 和 dnQualifier. 
    举例时间: 
    就使用确省值.如果没有缺省值, 那么该字段被忽略.用户如果输入 '.' ,也可以让该字段被忽略.用户输入的字节数必须在fieldName_min和fieldName_max之间. 不同的section还可能对输入的字符有特殊规定,比如必须是可打印字符.那么在本例里面, 程序的表现将如下: 
    首先把fieldName打印出来给用户以提示 
    please input ur name: 
    之后如果用户必须输入3到18之间的一个长度的字符串, 如果用户什么也不输入,那么就把fieldName_default里面的值"fordesign"作为该字段的值添入. 
    有的字段可以被重复使用.这就产生了一个问题, config文件是不允许同样的section文件里面有多于一个的相同的key的.其实这很容易解决,比如把它们的名字分别叫做 "1.organizationName", "2.organizationName" openssl在编译的时候封装了最必须的几个字段，比如commonName,countryName,localityName, organizationName,organizationUnitName, stateOrPrivinceName还增加了emailAddress surname, givenName initials 和 dnQualifier. 
    举例时间: 
    Examine and verify certificate request: 
    检查和验证CSR文件. 
    openssl req -in req.pem -text -verify -noout 
    做自己的私有密钥文件, 然后用这个文件生成CSR文件. 
    openssl genrsa -out key.pem 1024 
    openssl req -new -key key.pem -out req.pem 
    也可以一步就搞定: 
    openssl req -newkey rsa:1024 -keyout key.pem -out req.pem 
    做一个自签名的给Root CA用的证书: 
    openssl req -x509 -newkey rsa:1024 -keyout key.pem -out crt.pem 
    下面是与本指令有关的config文件中相关的部分的一个例子: 
    [ req ] 
    default_bits = 1024 
    default_keyfile = privkey.pem 
    distinguished_name = req_distinguished_name 
    attributes = req_attributes 
    x509_extensions = v3_ca 
    dirstring_type = nobmp 
    [ req_distinguished_name ] 
    countryName = Country Name (2 letter code) 
    countryName_default = AU 
    countryName_min = 2 
    countryName_max = 2 
    localityName = Locality Name (eg, city) 
    organizationalUnitName = Organizational Unit Name (eg, section) 
    commonName = Common Name (eg, YOUR name) 
    commonName_max = 64 
    emailAddress = Email Address 
    emailAddress_max = 40 
    [ req_attributes ] 
    challengePassword = A challenge password 
    challengePassword_min = 4 
    challengePassword_max = 20 
     [ v3_ca ] 
     subjectKeyIdentifier=hash 
    authorityKeyIdentifier=keyid:always,issuer:always 
    basicConstraints = CA:true 
    RANDFILE = $ENV::HOME/.rnd 
    [ req ] 
    default_bits = 1024 
    default_keyfile = keyfile.pem 
    distinguished_name = req_distinguished_name 
    attributes = req_attributes 
    prompt = no 
    output_password = mypass 
    [ req_distinguished_name ] 
    C = GB 
    ST = Test State or Province 
    L = Test Locality 
    O = Organization Name 
    OU = Organizational Unit Name 
    CN = Common Name 
    emailAddress = test@email.address 
    [ req_attributes ] 
    challengePassword = A challenge password 
    一般的PEM格式的CSR文件的开头和结尾一行如下 
    -----BEGIN CERTIFICATE REQUEST---- 
    -----END CERTIFICATE REQUEST---- 
    但个把变态软件和CA硬是需要CSR的文件要这样子: 
    -----BEGIN NEW CERTIFICATE REQUEST---- 
    -----END NEW CERTIFICATE REQUEST---- 
    用-newhdr就可以啦, 或者你自己手工加也中.openssl对俩种格式都承认. 
    openssl的config文件也可以用环境变量OPENSSL_CONF或者SSLEAY_CONF来指定.



