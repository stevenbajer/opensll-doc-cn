openssl简介－
指令cipher
说明：cipher就是加密算法的意思。ssl的cipher主要是对称加密算法和不对称加密算法的组合。 本指令是用来展示用于SSL加密算法的工具。它能够把所有openssl支持的加密算法按照一定规律排列（一般是加密强度）。这样可以用来做测试工具，决定使用什么加密算法。 


    用法： 
    openssl ciphers [-v] [-ssl2] [-ssl3] [-tls1] [cipherlist] 

COMMAND OPTIONS 
    -v 
    详细列出所有符合的cipher的所有细节。列出该cipher使用的ssl的版本，公共密钥交换算法，身份验证方法，对称加密算法以及哈希算法。还列出该算法是否可以出口。 

    算法出口？ 趁这个机会可以给大家来点革命教育。米国的加密算法研究是世界上最先进的，其国家安全局(NSA)在这方面的研究水平已经多次证明比"最先进水平"领先10到15年。他们的预算据说是每年200亿美圆。他们的数学家比你知道的还多，他们还是全世界最大的计算机硬件买家。DES就是他们最先弄出来的。到了70年代，IBM也有人在实现室弄出这个算法。都弄出来30年了，还使用的这么广泛。 
    该算法的最隐蔽的是一个叫S匣的东西，是一个常数矩阵。研究DES你就会知道这玩意。因为NSA和IBM都没有给出这个S匣的解释，所以大家都怀疑使用这个东西是否是NSA和IBM搞出来的后门？ 
    一直到了90年代，才有俩个以色列人发现了原因，这个是为了对付一种叫什么微分密码分析的破解法而如此设置的，对S匣的任何改动都将使微分密码分析比较容易的将DES给K掉。S匣不仅不是后门，还是最大限度的增加了加密强度。 

    这个故事教育我们，为了中国的崛起，还有很多路要走呐。 
   如果没有-v这个参数， 很多cipher可能重复出现，因为他们可以同时被不同版本的SSL协议使用。 

-ssl3 
    只列出SSLv3使用的ciphers 
     -ssl2 
    只列出SSLv2使用的ciphers 
    -tls1 
    只列出TLSv1使用的ciphers 
    -h, -? 
    打印帮助信息 
    cipherlist 
   列出一个cipher list的详细内容。一般都这么用： 
   openssl -v XXXXX 
   这个XXXXX就是cipher list.如果是空的话，那么XXXXX代表所有的cipher. 
   CIPHER LIST 的格式 
   cipher list由许多cipher string组成，由冒号，逗号或者空格分隔开。但一般最常用的是用冒号。  

cipher string又是什么？ 
   它可以仅仅包含一个cipher, 比如RC4-SHA. 
   它也可以仅仅包含一个加密算法，比如SHA, 那就表示所有用到SHA的cipher都得列出来。 
    你还可以使用三个符号来捏合各种不同的cipher,做出cipher string.这三个符号是 +, -, !。
    我想这个很好理解吧，
       (1) MD5+DES表示同时使用了这俩种算法的cipher，
       (2) !SHA就表示所有没有有用到SHA的cipher， 
       (3) IDEA-CBC就表示使用了IDEA而没有使用CBC的所有cipher.  

openssl还缺省的定义了一些通用的cipher string, 有： 
    DEFAULT: 缺省的cipher list. 
    ALL: 所有的cipher 
    HIGH, LOW, MEDIUM: 分别代表 高强度，中等强度和底强度的cipher list.具体一点就是对称加密算法的key的长度分别是 >128bit   <128bit和 ==128bit的cipher. 

    EXP, EXPORT, EXPORT40: 老米的垄断体现，前俩者代表法律允许出口的加密算法，包括40bit, 56bit长度的key的算法，后者表示只有40bit长度的key的加密算法。  

    eNULL, NULL: 表示不加密的算法。(那也叫加密算法吗？) 
    aNULL: 不提供身份验证的加密算法。目前只有DH一种。该算法很容易被监听者，路由器等中间设备攻击，所以不提倡使用。 

下表列出了SSL/TLS使用的cipher, 以及openssl里面如何表示这些cipher. 
    SSL v3.0 cipher suites OPENLLS表示方法 

SSL_RSA_WITH_NULL_MD5 NULL-MD5 

SSL_RSA_WITH_NULL_SHA NULL-SHA 

SSL_RSA_EXPORT_WITH_RC4_40_MD5 EXP-RC4-MD5 

SSL_RSA_WITH_RC4_128_MD5 RC4-MD5 

SSL_RSA_WITH_RC4_128_SHA RC4-SHA 

SSL_RSA_EXPORT_WITH_RC2_CBC_40_MD5 EXP-RC2-CBC-MD5 

SSL_RSA_WITH_IDEA_CBC_SHA IDEA-CBC-SHA 

SSL_RSA_EXPORT_WITH_DES40_CBC_SHA EXP-DES-CBC-SHA 

SSL_RSA_WITH_DES_CBC_SHA DES-CBC-SHA 

SSL_RSA_WITH_3DES_EDE_CBC_SHA DES-CBC3-SHA 

SSL_DH_DSS_EXPORT_WITH_DES40_CBC_SHA Not implemented. 

SSL_DH_DSS_WITH_DES_CBC_SHA Not implemented. 

SSL_DH_DSS_WITH_3DES_EDE_CBC_SHA Not implemented. 

SSL_DH_RSA_EXPORT_WITH_DES40_CBC_SHA Not implemented. 

SSL_DH_RSA_WITH_DES_CBC_SHA Not implemented. 

SSL_DH_RSA_WITH_3DES_EDE_CBC_SHA Not implemented. 

SSL_DHE_DSS_EXPORT_WITH_DES40_CBC_SHA EXP-EDH-DSS-DES-CBC-SHA 

SSL_DHE_DSS_WITH_DES_CBC_SHA EDH-DSS-CBC-SHA 

SSL_DHE_DSS_WITH_3DES_EDE_CBC_SHA EDH-DSS-DES-CBC3-SHA 

SSL_DHE_RSA_EXPORT_WITH_DES40_CBC_SHA EXP-EDH-RSA-DES-CBC-SHA 

SSL_DHE_RSA_WITH_DES_CBC_SHA EDH-RSA-DES-CBC-SHA 

SSL_DHE_RSA_WITH_3DES_EDE_CBC_SHA EDH-RSA-DES-CBC3-SHA 


SSL_DH_anon_EXPORT_WITH_RC4_40_MD5 EXP-ADH-RC4-MD5 

SSL_DH_anon_WITH_RC4_128_MD5 ADH-RC4-MD5 

SSL_DH_anon_EXPORT_WITH_DES40_CBC_SHA EXP-ADH-DES-CBC-SHA 

SSL_DH_anon_WITH_DES_CBC_SHA ADH-DES-CBC-SHA 

SSL_DH_anon_WITH_3DES_EDE_CBC_SHA ADH-DES-CBC3-SHA 

SSL_FORTEZZA_KEA_WITH_NULL_SHA Not implemented. 

SSL_FORTEZZA_KEA_WITH_FORTEZZA_CBC_SHA Not implemented. 

SSL_FORTEZZA_KEA_WITH_RC4_128_SHA Not implemented. 

TLS_RSA_EXPORT1024_WITH_DES_CBC_SHA EXP1024-DES-CBC-SHA 

TLS_RSA_EXPORT1024_WITH_RC4_56_SHA EXP1024-RC4-SHA 

TLS_DHE_DSS_EXPORT1024_WITH_DES_CBC_SHA EXP1024-DHE-DSS-DES-CBC-SHA 

TLS_DHE_DSS_EXPORT1024_WITH_RC4_56_SHA EXP1024-DHE-DSS-RC4-SHA 

TLS_DHE_DSS_WITH_RC4_128_SHA DHE-DSS-RC4-SHA 




TLS v1.0 cipher suites. 

TLS_RSA_WITH_NULL_MD5 NULL-MD5 

TLS_RSA_WITH_NULL_SHA NULL-SHA 

TLS_RSA_EXPORT_WITH_RC4_40_MD5 EXP-RC4-MD5 

TLS_RSA_WITH_RC4_128_MD5 RC4-MD5 

TLS_RSA_WITH_RC4_128_SHA RC4-SHA 

TLS_RSA_EXPORT_WITH_RC2_CBC_40_MD5 EXP-RC2-CBC-MD5 

TLS_RSA_WITH_IDEA_CBC_SHA IDEA-CBC-SHA 

TLS_RSA_EXPORT_WITH_DES40_CBC_SHA EXP-DES-CBC-SHA 

TLS_RSA_WITH_DES_CBC_SHA DES-CBC-SHA 

TLS_RSA_WITH_3DES_EDE_CBC_SHA DES-CBC3-SHA 

TLS_DH_DSS_EXPORT_WITH_DES40_CBC_SHA Not implemented. 

TLS_DH_DSS_WITH_DES_CBC_SHA Not implemented. 

TLS_DH_DSS_WITH_3DES_EDE_CBC_SHA Not implemented. 

TLS_DH_RSA_EXPORT_WITH_DES40_CBC_SHA Not implemented. 

TLS_DH_RSA_WITH_DES_CBC_SHA Not implemented. 

TLS_DH_RSA_WITH_3DES_EDE_CBC_SHA Not implemented. 

TLS_DHE_DSS_EXPORT_WITH_DES40_CBC_SHA EXP-EDH-DSS-DES-CBC-SHA 

TLS_DHE_DSS_WITH_DES_CBC_SHA EDH-DSS-CBC-SHA 

TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA EDH-DSS-DES-CBC3-SHA 

TLS_DHE_RSA_EXPORT_WITH_DES40_CBC_SHA EXP-EDH-RSA-DES-CBC-SHA 

TLS_DHE_RSA_WITH_DES_CBC_SHA EDH-RSA-DES-CBC-SHA 

TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA EDH-RSA-DES-CBC3-SHA 

TLS_DH_anon_EXPORT_WITH_RC4_40_MD5 EXP-ADH-RC4-MD5 

TLS_DH_anon_WITH_RC4_128_MD5 ADH-RC4-MD5 

TLS_DH_anon_EXPORT_WITH_DES40_CBC_SHA EXP-ADH-DES-CBC-SHA 

TLS_DH_anon_WITH_DES_CBC_SHA ADH-DES-CBC-SHA 

TLS_DH_anon_WITH_3DES_EDE_CBC_SHA ADH-DES-CBC3-SHA 


NOTES 
    DH算法由于老米没有允许人家使用，所有openssl都没有实现之。 

举例时间： 
    详细列出所有openssl支持的ciphers,包括那些eNULL ciphers: 
    openssl ciphers -v 'ALL:eNULL' 
    按加密强度列出所有加密算法: 
    openssl ciphers -v 'ALL:!ADH:@STRENGTH' 
    详细列出所有同时使用了3DES和RSA的ciphers 
    openssl ciphers -v '3DES:+RSA'

 

