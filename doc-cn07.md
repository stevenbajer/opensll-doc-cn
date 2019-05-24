openssl简介－指令ca

用途： 
    模拟CA行为的工具.有了它，你就是一个CA,不过估计是nobody trusted CA.可以用来给各种格式的CSR签名，用来产生和维护CRL(不记得CRL是什么了？去看证书那一章).他还维护着一个文本数据库，记录了所有经手颁发的证书及那些证书的状态。 
    用法： 
     openssl ca [-verbose] [-config filename] [-name section] [-gencrl] 

[-revoke file] [-crldays days] [-crlhours hours] [-crlexts section] 

[-startdate date] [-enddate date] [-days arg] [-md arg] [-policy arg] 

[-keyfile arg] [-key arg] [-passin arg] [-cert file] [-in file] 

[-out file] [-notext] [-outdir dir] [-infiles] [-spkac file] 

[-ss_cert file] [-preserveDN] [-batch] [-msie_hack] [-extensions section] 
     哇噻，好复杂也。不过用过GCC的人应该觉得这么点flag还是小case. 

-config filename 
    指定使用的configure文件。 

-in filename 
    要签名的CSR文件。 

-ss_cert filename 
    一个有自签名的证书，需要我们CA签名，就从这里输入文件名。 

-spkac filename 
    这一段实在没有看懂，也没兴趣，估计和SPKAC打交道可能性不大，奉送上英文原文。 
    a file containing a single Netscape signed public key and challenge and additional field values to be signed by the CA. 
SPKAC FORMAT 
    The input to the -spkac command line option is a Netscape signed public key and challenge. This will usually come from the KEYGEN tag in an HTML form to create a new private key. It is however possible to create SPKACs using the spkac utility. 
    The file should contain the variable SPKAC set to the value of the SPKAC and also the required DN components as name value pairs. If you need to include the same component twice then it can be preceded by a number and a . 
    -infiles 
    如果你一次要给几个CSR签名，就用这个来输入，但记得这个选项一定要放在最后。这个项后面的所有东东都被认为是CSR文件名参数。 
    -out filename 
    签名后的证书文件名。证书的细节也会给写进去。 
    -outdir directory 
    摆证书文件的目录。证书名就是该证书的系列号，后缀是.pem 
    -cert 
    CA本身的证书文件名 
    -keyfile filename 
    CA自己的私有密钥文件 
    -key password 
    CA的私有密钥文件的保护密码。 
    在有的系统上，可以用ps看到你输入的指令，所以这个参数要小心点用。 
    -passin arg 
    也是一个输入私有密钥保护文件的保护密码的一种方式，可以是文件名，设备名或者是有名管道。程序会把该文件的第一行作为密码读入。（也蛮危险的）。 
    -verbose 
    操作过程被详细printf出来 
    -notext 
    不要把证书文件的明文内容输出到文件中去。 
    -startdate date 
    指明证书的有效开始日期。格式是YYMMDDHHMMSSZ, 同ASN1的UTCTime结构相同。 
    -enddate date 
    指明证书的有效截止日期，格式同上。 
    -days arg 
    指明给证书的有效时间，比如365天。 
    -md alg 
    签名用的哈希算法，比如MD2, MD5等。 
    -policy arg 
    指定CA使用的策略。其实很简单，就是决定在你填写信息生成CSR的时候，哪些信息是我们必须的，哪些不是。看看config文件里面的policy这个item就明白了。 
    -msie_hack 
    为了和及其古老的证书版本兼容而做出的牺牲品，估计没人会用的，不解释了。 
    -preserveDN 
    和-msie_hack差不多的一个选项。 
    -batch 
    设置为批处理的模式，所有的CSR会被自动处理。 
    -extensions section 
    我们知道一般我们都用X509格式的证书，X509也有几个版本的。如果你在这个选项后面带的那个参数在config文件里有同样名称的key,那么就颁发X509V3证书，否则颁发X509v1证书。 
    还有几个关于CRL的选项，但我想一般很少人会去用。我自己也没兴趣去研究。 
    有兴趣的自己看看英文吧。 

CRL OPTIONS 

-gencrl 

this option generates a CRL based on information in the index file. 
      -crldays num 

the number of days before the next CRL is due. That is the days from 

now to place in the CRL nextUpdate field. 

-crlhours num 

the number of hours before the next CRL is due. 

-revoke filename 

a filename containing a certificate to revoke. 

-crlexts section 

the section of the configuration file containing CRL extensions to 

include. If no CRL extension section is present then a V1 CRL is created, 

if the CRL extension section is present (even if it is empty) then a V2 

CRL is created. The CRL extensions specified are CRL extensions and not 

CRL entry extensions. It should be noted that some software (for example 

Netscape) can't handle V2 CRLs. 

相信刚才大家都看到很多选项都和config文件有关,那么我们来解释一下config文件make install之后，openssl会生成一个全是缺省值的config文件:openssl.cnf.也长的很，贴出来有赚篇幅之嫌，xgh不屑。简单解释一下其中与CA有关的key.
与CA有关的key都在ca这个section之中。 
    [ ca ] 
    default_ca = CA_default 
    [ CA_default ] 
    dir = ./demoCA # Where everything is kept 
    certs = $dir/certs # Where the issued certs are kept 
    crl_dir = $dir/crl # Where the issued crl are kept 
    database = $dir/index.txt # database index file. 
    new_certs_dir = $dir/newcerts # default place for new certs. 
    certificate = $dir/cacert.pem # The CA certificate 
    serial = $dir/serial # The current serial number 
    crl = $dir/crl.pem # The current CRL 
    private_key = $dir/private/cakey.pem# The private key 
    RANDFILE = $dir/private/.rand # private random number file 
    x509_extensions = usr_cert # The extentions to add to the cert 
    # Extensions to add to a CRL. Note: Netscape communicator chokes on V2 CRLs 
    # so this is commented out by default to leave a V1 CRL. 
    # crl_extensions = crl_ext 
    default_days = 365 # how long to certify for 
    default_crl_days= 30 # how long before next CRL 
    default_md = md5 # which md to use. 
    preserve = no # keep passed DN ordering 
    # A few difference way of specifying how similar the request should look 
    # For type CA, the listed attributes must be the same, and the optional 
    # and supplied fields are just that :-) 
    policy = policy_match 
    # For the CA policy 
    [ policy_match ] 
    countryName = match 
    stateOrProvinceName = match 
    organizationName = match 
    organizationalUnitName = optional 
    commonName = supplied 
    emailAddress = optional 
    # At this point in time, you must list all acceptable 'object' 
    # types. 
    [ policy_anything ] 
    countryName = optional 
    stateOrProvinceName = optional 
    localityName = optional 
    organizationName = optional 
    organizationalUnitName = optional 
    commonName = supplied 
    emailAddress = optional 
    config文件里CA section里面的很多key都和命令行参数是对应的。 
    如果某个key后面标明mandatory,那就说明这个参数是必须提供的，无论你通过命令行还是通过config文件去提供。 

new_certs_dir 
    本key同命令行的 -outdir意义相同。(mandatory) 
    certificate 
    同命令行的 -cert意义相同。(mandatory) 
    private_key 
    同命令行-keyfile意义相同.(mandatory) 
    RANDFILE 
    指明一个用来读写时候产生random key的seed文件。具体意义在以后的RAND的API再给出解释。(不是我摆谱，我觉得重复解释没有必要) 
    default_days 
    意义和命令行的 -days相同。 
    default_startdate 
    意义同命令行的 -startdate相同。如果没有的话那么就使用产生证书的时间。 
    default_enddate 
    意义同命令行的 -enddate相同。(mandatory). 
    crl_extensions 
    preserve 
    default_crl_hours default_crl_days 
    CRL的东西.....自己都没弄懂..... 
    default_md 
    同命令行的-md意义相同. (mandatory) 
    database 
    记得index.txt是什么文件吗？不记得自己往前找。这个key就是指定index.txt的。初始化是空文件。 
    serialfile 
    指明一个txt文件，里面必须包含下一个可用的16进制数字，用来给下一个证书做系列号。(mandatory) 
    x509_extensions 
    意义同 -extensions相同。 
    msie_hack 
    意义同-msie_hack相同。 
    policy 
    意义同-policy相同。自己看看这一块是怎么回事。(mandatory) 
    [ policy_match ] 
    countryName = match 
    stateOrProvinceName = match 
    organizationName = match 
    organizationalUnitName = optional 
    commonName = supplied 
    emailAddress = optional 
    其实如果你做过CSR就会明白，这些项就是你做CSR时候填写的那些东西麻。 
    后面的"match", "supplied"等又是什么意思呢？"match"表示说明你填写的这一栏一定要和CA本身的证书里面的这一栏相同。supplied表示本栏必须，optional就表示本栏可以不填写。 
    举例时间到了： 
    注意，本例中我们先要在 $OPENSSL/misc下面运行过CA.sh -newca，建立好相应的目录，所有需要的文件，包括CA的私有密钥文件，证书文件，系列号文件，和一个空的index文件。并且文件都已经各就各位。放心把，产生文件和文件就位都由CA.sh搞定，你要做的就是运行CA.sh -nweca就行了，甚至在你的系列号文件中还有个01,用来给下一个证书做系列号。 
    给一个CSR签名： 
    openssl ca -in req.pem -out newcert.pem 
    给一个CSR签名, 产生x509v3证书： 
    openssl ca -in req.pem -extensions v3_ca -out newcert.pem 
    同时给数个CSR签名： 
    openssl ca -infiles req1.pem req2.pem req3.pem 
    注意： 
    index.txt文件是整个处理过程中很重要的一部分，如果这玩意坏了，很难修复。理论上根据已经颁发的证书和当前的CRL当然是有办法修复的啦，但openssl没提供这个功能。:( 
    openssl还有俩大类指令: crl, crl2pkcs7, 都是和CRL有关的， 
    由于我们对这个没有兴趣，所以这俩大类不做翻译和解释。

 

