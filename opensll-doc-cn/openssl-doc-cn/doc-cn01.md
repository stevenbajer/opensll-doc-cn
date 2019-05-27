openssl简介--证书

发信站:BBS水木清华站(FriNov1020:29:282000)

引用请指明原作译者fordesign@21cn.com

二证书

     证书就是数字化的文件，里面有一个实体(网站，个人等)的公共密钥和其他的属性，如名称等。该公共密钥只属于某一个特定的实体，它的作用是防止一个实体假装成另外一个实体。

证书用来保证不对称加密算法的合理性。想想吧，如果没有证书记录，那么假设某俩人A与B的通话过程如下：

这里假设A的publickey是K1,privatekey是K2 ，                     B的publickey是K3,privatekey是K4

xxxxxx(kn)表示用kn加密过的一段文字xxxxxx

A-----〉hello(plaintext)-------------〉B
A〈---------hello(plaintext)〈---------B
A〈---------Bspublickey〈------------B
A---------〉spublickey(K1)--------〉B
......

如果C想假装成B,那么步骤就和上面一样。
A-----〉hello(plaintext)-------------〉C
A〈---------hello(plaintext)〈---------C

注意下一步，因为A没有怀疑C的身份，所以他理所当然的接受了C的publickey,并且使用这个key来继续下面的通信。

A〈---------Cspublickey〈------------C
A---------〉Aspublickey(K1)--------〉C
......

这样的情况下A是没有办法发觉C是假的。如果A在通话过程中要求取得B的证书，并且验证证书里面记录的名字，如果名字和B的名字不符合，就可以发现对方不是B.验证B的名字通过再从证书里面提取B的公用密钥，继续通信过程。


那么，如果证书是假的怎么办？或者证书被修改过了怎么办？慢慢看下来吧。


证书最简单的形式就是只包含有证书拥有者的名字和公用密钥。当然现在用的证书没这么简单，里面至少还有证书过期的deadline,颁发证书的机构名称，证书系列号，和一些其他可选的信息。最重要的是，它包含了证书颁发机构(certificationauthority简称CA)的签名信息。

我们现在常用的证书是采用X.509结构的，这是一个国际标准证书结构。任何遵循该标准的应用程序都可以读，写X509结构的证书。

通过检查证书里面的CA的名字，和CA的签名，就知道这个证书的确是由该CA签发的然后，你就可以简单证书里面的接收证书者的名字，然后提取公共密钥。这样做建立的基础是，你信任该CA,认为该CA没有颁发错误的证书。

CA是第三方机构，被你信任，由它保证证书的确发给了应该得到该证书的人。CA自己有一个庞大的publickey数据库，用来颁发给不同的实体。

这里有必要解释一下，CA也是一个实体，它也有自己的公共密钥和私有密钥，否则怎么做数字签名？它也有自己的证书，你可以去它的站点down它的证书得到它的公共密钥。

一般CA的证书都内嵌在应用程序中间。不信你打开你的IE,在internet选项里面选中"内容",点击"证书",看看那个"中间证书发行机构"和"委托根目录发行机构",是不是有一大堆CA的名称？也有时CA的证书放在安全的数据库里面。

当你接受到对方的证书的时候，你首先会去看该证书的CA,然后去查找自己的CA证书数据库，看看是否找的到，找不到就表示自己不信任该CA,那么就告吹本次连接。找到了的话就用该CA的证书里面的公用密钥去检查CA在证书上的签名。

这里又有个连环的问题，我怎么知道那个CA的证书是属于那个CA的？人家不能造假吗？

解释一下吧。CA也是分级别的。最高级别的CA叫RootCAs,其他cheap一点的CA的证书由他们来颁发和签名。这样的话，最后的保证就是：我们信任RootCAs.那些有RootCAs签名过的证书的CA就可以来颁发证书给实体或者其他CA了。

你不信任RootCAs?人民币由中国人民银行发行，运到各个大银行，再运到地方银行，你从地方银行取人民币的时候不信任发行它的中国人民银行吗？RootCAs都是很权威的机构，没有必要担心他们的信用。

那RootCAs谁给签名?他们自己给自己签名,叫自签名.

说了这么多，举个certificate的例子吧,对一些必要的item解释一下。

CertificateExample
Certificate:
Data:
Version:1(0x0)
SerialNumber://系列号
02:41:00:00:16
SignatureAlgorithm:md2WithRSAEncryption//CA同志的数字签名的算法
Issuer:C=US,O=RSADataSecurity,Inc.,OU=Commercial//CA自报家门
Certification
Authority
Validity
NotBefore:Nov418:58:341994GMT//证书的有效期
NotAfter:Nov318:58:341999GMT
Subject:C=US,O=RSADataSecurity,Inc.,OU=Commercial
CertificationAuthority
SubjectPublicKeyInfo:
PublicKeyAlgorithm:rsaEncryption
RSAPublicKey:(1000bit)
Modulus(1000bit):
00:a4:fb:81:62:7b:ce:10:27:dd:e8:f7:be:6c:6e:
c6:70:99:db:b8:d5:05:03:69:28:82:9c:72:7f:96:
3f:8e:ec:ac:29:92:3f:8a:14:f8:42:76:be:bd:5d:
03:b9:90:d4:d0:bc:06:b2:51:33:5f:c4:c2:bf:b6:
8b:8f:99:b6:62:22:60:dd:db:df:20:82:b4:ca:a2:
2f:2d:50:ed:94:32:de:e0:55:8d:d4:68:e2:e0:4c:
d2:cd:05:16:2e:95:66:5c:61:52:38:1e:51:a8:82:
a1:c4:ef:25:e9:0a:e6:8b:2b:8e:31:66:d9:f8:d9:
fd:bd:3b:69:d9:eb
Exponent:65537(0x10001)
SignatureAlgorithm:md2WithRSAEncryption
76:b5:b6:10:fe:23:f7:f7:59:62:4b:b0:5f:9c:c1:68:bc:49:
bb:b3:49:6f:21:47:5d:2b:9d:54:c4:00:28:3f:98:b9:f2:8a:
83:9b:60:7f:eb:50:c7:ab:05:10:2d:3d:ed:38:02:c1:a5:48:
d2:fe:65:a0:c0:bc:ea:a6:23:16:66:6c:1b:24:a9:f3:ec:79:
35:18:4f:26:c8:e3:af:50:4a:c7:a7:31:6b:d0:7c:18:9d:50:
bf:a9:26:fa:26:2b:46:9c:14:a9:bb:5b:30:98:42:28:b5:4b:
53:bb:43:09:92:40:ba:a8:aa:5a:a4:c6:b6:8b:57:4d:c5

其实这是我们看的懂的格式的证书内容，真正的证书都是加密过了的，如下：

-----BEGINCERTIFICATE-----

MIIDcTCCAtqgAwIBAgIBADANBgkqhkiG9w0BAQQFADCBiDELMAkGA1UEBhMCQ0gx

EjAQBgNVBAgTCWd1YW5nZG9uZzESMBAGA1UEBxMJZ3Vhbmd6aG91MREwDwYDVQQK

Ewhhc2lhaW5mbzELMAkGA1UECxMCc3cxDjAMBgNVBAMTBWhlbnJ5MSEwHwYJKoZI

hvcNAQkBFhJmb3JkZXNpZ25AMjFjbi5jb20wHhcNMDAwODMwMDc0MTU1WhcNMDEw

ODMwMDc0MTU1WjCBiDELMAkGA1UEBhMCQ0gxEjAQBgNVBAgTCWd1YW5nZG9uZzES

MBAGA1UEBxMJZ3Vhbmd6aG91MREwDwYDVQQKEwhhc2lhaW5mbzELMAkGA1UECxMC

c3cxDjAMBgNVBAMTBWhlbnJ5MSEwHwYJKoZIhvcNAQkBFhJmb3JkZXNpZ25AMjFj

bi5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAMDYArTAhLIFacYZwP30

Zu63mAkgpAjVHaIsIEJ6wySIZl2THEHjJ0kS3i8lyMqcl7dUFcAXlLYi2+rdktoG

jBQMOtOHv1/cmo0vzuf38+NrAZSZT9ZweJfIlp8W9uyz8Dv5hekQgXFg/l3L+HSx

wNvQalaOEw2nyf45/np/QhNpAgMBAAGjgegwgeUwHQYDVR0OBBYEFKBL7xGeHQSm

ICH5wBrOiqNFiildMIG1BgNVHSMEga0wgaqAFKBL7xGeHQSmICH5wBrOiqNFiild

oYGOpIGLMIGIMQswCQYDVQQGEwJDSDESMBAGA1UECBMJZ3Vhbmdkb25nMRIwEAYD

VQQHEwlndWFuZ3pob3UxETAPBgNVBAoTCGFzaWFpbmZvMQswCQYDVQQLEwJzdzEO

MAwGA1UEAxMFaGVucnkxITAfBgkqhkiG9w0BCQEWEmZvcmRlc2lnbkAyMWNuLmNv

bYIBADAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4GBAGQa9HK2mixM7ML7

0jZr1QJUHrBoabX2AbDchb4Lt3qAgPOktTc3F+K7NgB3WSVbdqC9r3YpS23RexU1

aFcHihDn73s+PfhVjpT8arC1RQDg9bDPvUUYphdQC0U+HF72/CvxGCTqpnWiqsgw

xqeog0A8H3doDrffw8Zb7408+Iqf

-----ENDCERTIFICATE-----

证书都是有寿命的。就是上面的那个NotBefore和NotAfter之间的日子。过期的证书，如果没有特殊原因，都要摆在证书回收列(certificaterevocationlist)里面.证书回收列，英文缩写是CRL.比如一个证书的key已经被破了，或者证书拥有者没有权力再使用该证书，该证书就要考虑作废。CRL详细记录了所有作废的证书。

CRL的缺省格式是PEM格式。当然也可以输出成我们可以读的文本格式。下面有个CRL的例子。

-----BEGINX509CRL-----

MIICjTCCAfowDQYJKoZIhvcNAQECBQAwXzELMAkGA1UEBhMCVVMxIDAeBgNVBAoT

F1JTQSBEYXRhIFNlY3VyaXR5LCBJbmMuMS4wLAYDVQQLEyVTZWN1cmUgU2VydmVy

IENlcnRpZmljYXRpb24gQXV0aG9yaXR5Fw05NTA1MDIwMjEyMjZaFw05NTA2MDEw

MDAxNDlaMIIBaDAWAgUCQQAABBcNOTUwMjAxMTcyNDI2WjAWAgUCQQAACRcNOTUw

MjEwMDIxNjM5WjAWAgUCQQAADxcNOTUwMjI0MDAxMjQ5WjAWAgUCQQAADBcNOTUw

MjI1MDA0NjQ0WjAWAgUCQQAAGxcNOTUwMzEzMTg0MDQ5WjAWAgUCQQAAFhcNOTUw

MzE1MTkxNjU0WjAWAgUCQQAAGhcNOTUwMzE1MTk0MDQxWjAWAgUCQQAAHxcNOTUw

MzI0MTk0NDMzWjAWAgUCcgAABRcNOTUwMzI5MjAwNzExWjAWAgUCcgAAERcNOTUw

MzMwMDIzNDI2WjAWAgUCQQAAIBcNOTUwNDA3MDExMzIxWjAWAgUCcgAAHhcNOTUw

NDA4MDAwMjU5WjAWAgUCcgAAQRcNOTUwNDI4MTcxNzI0WjAWAgUCcgAAOBcNOTUw

NDI4MTcyNzIxWjAWAgUCcgAATBcNOTUwNTAyMDIxMjI2WjANBgkqhkiG9w0BAQIF

AAN+AHqOEJXSDejYy0UwxxrH/9+N2z5xu/if0J6qQmK92W0hW158wpJg+ovV3+wQ

wvIEPRL2rocL0tKfAsVq1IawSJzSNgxG0lrcla3MrJBnZ4GaZDu4FutZh72MR3Gt

JaAL3iTJHJD55kK2D/VoyY1djlsPuNh6AEgdVwFAyp0v

-----ENDX509CRL-----

下面是文本格式的CRL的例子。

ThefollowingisanexampleofaCRLintextformat:

issuer=/C=US/O=RSADataSecurity,Inc./OU=SecureServerCertification

Authority

lastUpdate=May202:12:261995GMT

nextUpdate=Jun100:01:491995GMT

revoked:serialNumber=027200004CrevocationDate=May202:12:261995GMT

revoked:serialNumber=0272000038revocationDate=Apr2817:27:211995GMT

revoked:serialNumber=0272000041revocationDate=Apr2817:17:241995GMT

revoked:serialNumber=027200001ErevocationDate=Apr800:02:591995GMT

revoked:serialNumber=0241000020revocationDate=Apr701:13:211995GMT

revoked:serialNumber=0272000011revocationDate=Mar3002:34:261995GMT

revoked:serialNumber=0272000005revocationDate=Mar2920:07:111995GMT

revoked:serialNumber=024100001FrevocationDate=Mar2419:44:331995GMT

revoked:serialNumber=024100001ArevocationDate=Mar1519:40:411995GMT

revoked:serialNumber=0241000016revocationDate=Mar1519:16:541995GMT

revoked:serialNumber=024100001BrevocationDate=Mar1318:40:491995GMT

revoked:serialNumber=024100000CrevocationDate=Feb2500:46:441995GMT

revoked:serialNumber=024100000FrevocationDate=Feb2400:12:491995GMT

revoked:serialNumber=0241000009revocationDate=Feb1002:16:391995GMT

revoked:serialNumber=0241000004revocationDate=Feb117:24:261995GMT

总结一下X.509证书是个什么东东吧。它实际上是建立了公共密钥和某个实体之间联系的数字化的文件。它包含的内容有：

版本信息,X.509也是有三个版本的。

系列号
证书接受者名称
颁发者名称
证书有效期
公共密钥
一大堆的可选的其他信息
CA的数字签名

证书由CA颁发，由CA决定该证书的有效期，由该CA签名。每个证书都有唯一的系列号。证书的系列号和证书颁发者来决定某证书的唯一身份。

openssl有四个验证证书的模式。你还可以指定一个callback函数，在验证证书的时候会自动调用该callback函数。这样可以自己根据验证结果来决定应用程序的行为。具体的东西在以后的章节会详细介绍的。

openssl的四个验证证书模式分别是：

SSL_VERIFY_NONE:完全忽略验证证书的结果。当你觉得握手必须完成的话，就选用这个选项。其实真正有证书的人很少,尤其在中国。那么如果SSL运用于一些免费的服务，比如email的时候，我觉得server端最好采用这个模式。

SSL_VERIFY_PEER:希望验证对方的证书。不用说这个是最一般的模式了.对client来说，如果设置了这样的模式，验证server的证书出了任何错误，SSL握手都告吹.对server来说,如果设置了这样的模式,client倒不一定要把自己的证书交出去。如果client没有交出证书，server自己决定下一步怎么做。

SSL_VERIFY_FAIL_IF_NO_PEER_CERT:这是server使用的一种模式，在这种模式下，server会向client要证书。如果client不给，SSL握手告吹。

SSL_VERIFY_CLIENT_ONCE：这是仅能使用在sslsessionrenegotiation阶段的一种方式。什么是SSLsessionrenegotiation?以后的章节再解释。我英文差点，觉得这个词组也很难翻译成相应的中文。以后的文章里，我觉得很难直接翻译的单词或词组，都会直接用英文写出来。如果不是用这个模式的话,那么在regegotiation的时候，client都要把自己的证书送给server,然后做一番分析。这个过程很消耗cpu时间的，而这个模式则不需要client在regotiation的时候重复送自己的证书了。



