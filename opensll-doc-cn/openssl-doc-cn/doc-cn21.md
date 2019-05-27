openssl简介－指令s_server
 用法： 

openssl s_server [-accept port] [-context id] [-verify depth] 

[-Verify depth] [-cert filename] [-key keyfile] [-dcert filename] 

[-dkey keyfile] [-dhparam filename] [-nbio] [-nbio_test] [-crlf] 

[-debug] [-state] [-CApath directory] [-CAfile filename] [-nocert] 

[-cipher cipherlist] [-quiet] [-no_tmp_rsa] [-ssl2] [-ssl3] [-tls1] 

[-no_ssl2] [-no_ssl3] [-no_tls1] [-no_dhe] [-bugs] [-hack] [-www] 

[-WWW] [-engine id] 

说明： 
    和s_client是反义词， 模拟一个实现了SSL的server. 


    OPTIONS 
    -accept port 
    监听的TCP端口。缺省为4433. 
    -context id 
    设置SSL context的id, 可以设置为任何值。SSL context是什么？编程的章节会详细介绍的。你也可以不set这个option, 有缺省的给你用的。 
    -cert certname 
    使用的证书文件名。缺省使用 ./server.pem 
    -key keyfile 
    使用的私有密钥文件。如果没有指定，那么证书文件会被使用。???? 
    The private key to use. If not specified then the certificate 
    file will be used. 
    -dcert filename, -dkey keyname 
    指定一个附加的证书文件和私有密钥文件。不同的cipher需要不同的证书和 私有密钥文件。这个不同的cipher主要指cipher里面的不对称加密算法不同  比如基于RSA的cipher需要的是RSA的私有密钥文件和证书,而基于DSA的算法  则需要的是DSA的私有密钥文件和证书.这个option可以让这样我们的server同时支持俩种算法的cipher成为可能。 
    -nocert 
    如果server不想使用任何证书，set这个option. 
    目前只有anonymous DH算法有需要这么做。 
    -dhparam filename 
    使用的DH参数文件名。如果没有set, 那么server会试图去从证书文件里面获得这些参数。如果证书里面没有这么参数，一些hard code的参数就被调用。 
    -nodhe 
    禁止使用基于EDH的cipher. 
    -no_tmp_rsa 
    现在的出口cipher有时会使用临时RSA密钥。那就是说每次对话的时候临时生成密钥对。本optio就是用来禁止这种情况的。 
    -verify depth, -Verify depth 
    意义和s_client的这个option一样，但同时表示必须验证client的证书。不记得server对client的证书验证是可以选的吗？-verify表示向client要求证书，但client还是可以选择不发送证书，-Verify表示一定要client的证书验证，否则握手告吹。 
    -CApath directory 
    -CAfile file 
    -state 
    -debug 
    -nbio_test 
    -nbio 
    -crlf 
    -quiet 
    -ssl2, -ssl3, -tls1, -no_ssl2, -no_ssl3, -no_tls1 
    -bugs 
    -cipher cipherlist 
    这些option于s_client的同名option意义相同。 
    下面俩个指令模拟一个简单的http server. 
    -www 
    当client连接上来的时候，发回一个网页，内容就是SSL握手的一些内容。 
    -WWW 
    用来把具体某个文件当网页发回给client的请求。比如client的URL请求是 https://myhost/page.html ,就把 ./page.html发回给client.如果没有set -www, -WWW这俩个option, 当一个ssl client连接上来的话它所发过来的任何东西都会显示出来，你在终端输入的任何东西都会发回 给client.你可以通过在终端输入的行的第一个字母控制一些行为 
    q: 
    中断当前连接，但不关闭server. 
    Q 
    中断当前连接，退出程序。 
    r 
    进行renegotiate行为。 
    R 
    进行renegotiate行为, 并且要求client的证书 。 
    P 
    在TCP层直接送一些明文。这会使client认为我们没有按协议的游戏规则进行通信而断开连接。 
    S 
    打印出session-cache的状态信息。session-cache在编程章节会详细介绍。 
    NOTES 
    用于调试ssl client. 
    下一条指令用来模拟一个小的http server, 监听443端口。 
    openssl s_server -accept 443 -www 
    session的参数可以用sess_id指令打印。 
    我对这条指令实在没有兴趣，一般使用openssl都是用做server, 没有机会调试client.我甚至没有用过这个指令。



