openssl简介－指令rsautl
    用法： 

openssl rsautl [-in file] [-out file] [-inkey file] [-pubin] [-certin] 

[-sign] [-verify] [-encrypt] [-decrypt] [-pkcs] [-ssl] [-raw] [-hexdump] 

[-asn1parse] 


    描述： 
    本指令能够使用RSA算法签名，验证身份， 加密/解密数据。 

OPTIONS 
    -in filename 
    指定输入文件名。缺省为标准输入。 
    -out filename 
    指定输入文件名， 缺省为标准输出。 
    -inkey file 
    指定我们的私有密钥文件， 格式必须是RSA私有密钥文件。 
    -pubin 
    指定我们的公共密钥文件。说真的我还真不知道RSA的公共密钥文件有什么用，一般公共密钥都是放在证书里面的。 
    -certin 
    指定我们的证书文件了。 
    -sign 
    给输入的数据签名。需要我们的私有密钥文件。 
    -verify 
    对输入的数据进行验证。 
    -encrypt 
    用我们的公共密钥对输入的数据进行加密。 
    -decrypt 
    用RSA的私有密钥对输入的数据进行解密。 
    -pkcs, -oaep, -ssl, -raw 
   采用的填充模式， 上述四个值分别代表：PKCS#1.5(缺省值), PKCS#1 OAEP, SSLv2里面特定的填充模式，或者不填充。如果要签名，只有-pkcs和-raw可以使用. 
    -hexdump 
    用十六进制输出数据。 
    -asn1parse 
    对输出的数据进行ASN1分析。看看指令asn1parse吧。该指令一般和-verify一起用的时候威力大。 
    本指令加密数据的时候只能加密少量数据，要加密大量数据，估计要调API.我也没试过写RSA加密解密的程序来玩。 
    举例时间： 
    用私有密钥对某文件签名： 
    openssl rsautl -sign -in file -inkey key.pem -out sig 
    注意哦， 文件真的不能太大， 这个不能太大意思是必须很小。 
    文件大小最好不要大过73。绝对不能多过150，多了就会出错。 
    这个工具真是用来玩的 

对签名过的数据进行验证，得到原来的数据。 
    openssl rsautl -verify -in sig -inkey key.pem 
    检查原始的签名过的数据： 
    openssl rsautl -verify -in sig -inkey key.pem -raw -hexdump 

0000 - 00 01 ff ff ff ff ff ff-ff ff ff ff ff ff ff ff ................ 

0010 - ff ff ff ff ff ff ff ff-ff ff ff ff ff ff ff ff ................ 

0020 - ff ff ff ff ff ff ff ff-ff ff ff ff ff ff ff ff ................ 

0030 - ff ff ff ff ff ff ff ff-ff ff ff ff ff ff ff ff ................ 

0040 - ff ff ff ff ff ff ff ff-ff ff ff ff ff ff ff ff ................ 

0050 - ff ff ff ff ff ff ff ff-ff ff ff ff ff ff ff ff ................ 

0060 - ff ff ff ff ff ff ff ff-ff ff ff ff ff ff ff ff ................ 

0070 - ff ff ff ff 00 68 65 6c-6c 6f 20 77 6f 72 6c 64 .....hello world 
    很明显，这是PKCS#1结构：使用0xff填充模式。 
    配合指令asn1parse,可以分析签名的证书，我们在req指令里说了怎么做自签名的证书了，现在来分析一下先。 
    openssl asn1parse -in pca-cert.pem 

0:d=0 hl=4 l= 742 cons: SEQUENCE 

4:d=1 hl=4 l= 591 cons: SEQUENCE 

8:d=2 hl=2 l= 3 cons: cont [ 0 ] 

10:d=3 hl=2 l= 1 prim: INTEGER :02 

13:d=2 hl=2 l= 1 prim: INTEGER :00 

16:d=2 hl=2 l= 13 cons: SEQUENCE 

18:d=3 hl=2 l= 9 prim: OBJECT :md5WithRSAEncryption 

29:d=3 hl=2 l= 0 prim: NULL 

31:d=2 hl=2 l= 92 cons: SEQUENCE 

33:d=3 hl=2 l= 11 cons: SET 

35:d=4 hl=2 l= 9 cons: SEQUENCE 

37:d=5 hl=2 l= 3 prim: OBJECT :countryName 

42:d=5 hl=2 l= 2 prim: PRINTABLESTRING :AU 

.... 

599:d=1 hl=2 l= 13 cons: SEQUENCE 

601:d=2 hl=2 l= 9 prim: OBJECT :md5WithRSAEncryption 

612:d=2 hl=2 l= 0 prim: NULL 

614:d=1 hl=3 l= 129 prim: BIT STRING 

最后一行BIT STRING就是实际的签名。我们可以这样子捏它出来： 
    openssl asn1parse -in pca-cert.pem -out sig -noout -strparse 614 
   还可以这样子把公共密钥给弄出来： 
    openssl x509 -in test/testx509.pem -pubkey -noout >;pubkey.pem 
    我们也可以这样子分析签名： 
    openssl rsautl -in sig -verify -asn1parse -inkey pubkey.pem -pubin 
    0:d=0 hl=2 l= 32 cons: SEQUENCE 

2:d=1 hl=2 l= 12 cons: SEQUENCE 

4:d=2 hl=2 l= 8 prim: OBJECT :md5 

14:d=2 hl=2 l= 0 prim: NULL 

16:d=1 hl=2 l= 16 prim: OCTET STRING 

0000 - f3 46 9e aa 1a 4a 73 c9-37 ea 93 00 48 25 08 b5 .F...Js.7...H%.. 

这是经过分析后的ASN1结构。可以看出来使用的哈希算法是md5. (很抱歉，我自己试这一行的时候输出结果却完全不同。 
    0:d=0 hl=2 l= 120 cons: appl [ 24 ] 
    length is greater than 18 
    完全没有办法看出那里有写哈希算法。) 
    证书里面的签名部分可以这么捏出来： 
    openssl asn1parse -in pca-cert.pem -out tbs -noout -strparse 4 
    这样得到他的哈希算法的细节： 
    openssl md5 -c tbs 
    MD5(tbs)= f3:46:9e:aa:1a:4a:73:c9:37:ea:93:00:48:25:08:b5



