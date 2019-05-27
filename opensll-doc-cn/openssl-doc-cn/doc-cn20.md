openssl简介－指令s_client

用法： 

openssl s_client [-connect host:port>;] [-verify depth] [-cert filename] 

[-key filename] [-CApath directory] [-CAfile filename] [-reconnect] 

[-pause] [-showcerts] [-debug] [-nbio_test] [-state] [-nbio] [-crlf] 

[-ign_eof] [-quiet] [-ssl2] [-ssl3] [-tls1] [-no_ssl2] [-no_ssl3] 

[-no_tls1] [-bugs] [-cipher cipherlist] 

描述： 
    用于模拟一个普通的SSL/TLS client, 对于调试和诊断SSL server很有用。 

OPTIONS 
    -connect host:port 
    这个不用解释了吧， 连接的ip:port. 
    -cert certname 
    使用的证书文件。如果server不要求要证书，这个可以省略。 
    -key keyfile 
    使用的私有密钥文件 
    -verify depth 
    指定验证深度。记得CA也是分层次的吧？如果对方的证书的签名CA不是Root CA,那么你可以再去验证给该CA的证书签名的CA， 一直到Root CA. 目前的验证操作即使这条CA链上的某一个证书验证有问题也不会影响对更深层的CA的身份的验证。所以整个CA链上的问题都可以检查出来。当然CA的验证出问题并不会直接造成连接马上断开，好的应用程序可以让你根据验证结果决定下一步怎么走。 
    -CApath directory 
    一个目录。里面全是CA的验证资料，该目录必须是"哈希结构". verify指令里会详细说明。在建立client的证书链的时候也有用到这个指令。 
    -CAfile file 
    某文件，里面是所有你信任的CA的证书的内容。当你要建立client的证书链的时候也需要用到这个文件。 
    -reconnect 
    使用同样的session-id连接同一个server五次，用来测试server的session缓冲功能是否有问题。 
    -pause 
    每次读写操作后都挺顿一秒。 
    -showcerts 
    显示整条server的证书的CA的证书链。否则只显示server的证书。 
    -prexit 
    当程序退出的时候打印session的信息。即使连接失败，也会打印出调试信息。一般如果连接成功的话，调试信息将只被打出来一次。本option比较有用，因为在一次SSL连接中，cipher也可能改变，或者连接可能失败。要注意的是：有时候打印出来的东西并不一定准确。(这样也行？？eric, 言重了.) 
    -state 
    打印SSL session的状态， ssl也是一个协议，当然有状态。 
    -debug 
    打印所有的调试信息。 
    -nbio_test 
    检查非阻塞socket的I/O运行情况。 
    -nbio 
    使用非阻塞socket 
    -crlf 
    回把你在终端输入的换行回车转化成/r/n送出去。 
    -ign_eof 
   当输入文件到达文件尾的时候并不断开连接。 
   -quiet 
   不打印出session和证书的信息。同时会打开-ign_eof这个option. 
   -ssl2, -ssl3, -tls1, -no_ssl2, -no_ssl3, -no_tls1 
   选择用什么版本的协议。很容易理解，不用多解释了吧。 
   注意，有些很古老的server就是不能处理TLS1, 所以这个时候要关掉TLS1.n. 
   -bugs 
   SSL/TLS有几处众所周知的bug, set了这个option使出错的可能性缩小。 
   -cipher cipherlist 
   由我们自己来决定选用什么cipher，尽管是由server来决定使用什么cipher,但它一般都会采用我们送过去的cipher列表里的第一个cipher. 
    有哪些cipher可用？指令cipher对这个解释的更清楚。 
    一旦和某个SSL server建立连接之后，所有从server得到的数据都会被打印出来，所有你在终端上输入的东西也会被送给server. 这是人机交互式的。这时候不能set -quiet和 -ign_eof这俩个option。如果输入的某行开头字母是R,那么在这里session会renegociate, 如果输入的某行开头是Q, 那么连接会被断开。你完成整个输入之后连接也会被断开。 
    If a connection is established with an SSL server then any data received from the server is displayed and any key presses will be sent to the server. When used interactively (which means neither -quiet nor -ign_eof have been given), the session will be renegociated if the line begins with an R, and if the line begins with a Q or if end of file is reached, the connection will be closed down. 
    本指令主要是来debug一个SSL server的。如果想连接某个SSL HTTP server,输入下一条指令： 
   openssl s_client -connect servername:443 
   如果连接成功，你可以用HTTP的指令，比如"GET /"什么的去获得网页了。 
    如果握手失败，原因可能有以下几种： 
    1. server需要验证你的证书，但你没有证书 
    2.如果肯定不是原因1, 那么就慢慢一个一个set以下几个option 
    -bugs, -ssl2, -ssl3, -tls1, -no_ssl2, -no_ssl3, -no_tls1 
    这可能是因为对方的server处理SSL有bug. 
    有的时候，client会报错：没有证书可以使用，或者供选择的证书列表是空的。这一般是因为Server没有把给你签名的CA的名字列进它自己认为可以信任的CA列表,你可以用检查一下server的信任CA列表。有的http server只在 client给出了一个URL之后才验证client的证书，这中情况下要set -prexit这个option, 并且送给server一个页面请求。 
    即使使用-cert指明使用的证书，如果server不要求验证client的证书，那么该证书也不会被验证。所以不要以为在命令行里加了-cert 的参数又连接成功就代表你的证书没有问题。 
    如果验证server的证书没有问题，就可以set -showcerts来看看server的证书的CA链了。 
    其实这个工具并不好用， 自己写一个client的会方便很多。 
    举例时间： 
    注意，中间的pop3协议的指令是我通过终端输入的。其他都是程序输出的对话 
    过程。具体的每行意义不用解释了。


openssl s_client -key server.key -verify 1 -showcerts -prexit -state \ 
    -crlf -connect 127.0.0.1:5995 
    verify depth is 1 
    CONNECTED(00000003) 
    SSL_connect:before/connect initialization 
    SSL_connect:SSLv2/v3 write client hello A 
    SSL_connect:SSLv3 read server hello A 
    depth=0 /C=AU/ST=Some-State/L=gz/O=ai ltd/OU=sw/CN=fordesign/ 
    Email=xxx@xxx.xom 
    verify error:num=20:unable to get local issuer certificate 
    verify return:1 
    depth=0 /C=AU/ST=Some-State/L=gz/O=ai ltd/OU=sw/CN=fordesign/ 
    Email=xxx@xxx.xom 
    verify error:num=27:certificate not trusted 
    verify return:1 
    depth=0 /C=AU/ST=Some-State/L=gz/O=ai ltd/OU=sw/CN=fordesign/ 
    Email=xxx@xxx.xom 
    verify error:num=21:unable to verify the first certificate 
    verify return:1 
    SSL_connect:SSLv3 read server certificate A 
    SSL_connect:SSLv3 read server done A 
    SSL_connect:SSLv3 write client key exchange A 
    SSL_connect:SSLv3 write change cipher spec A 
    SSL_connect:SSLv3 write finished A 
    SSL_connect:SSLv3 flush data 
    SSL_connect:SSLv3 read finished A 
    Certificate chain 
    0 s:/C=AU/ST=Some-State/L=gz/O=ai ltd/OU=sw/CN=fordesign/Email=xxx@xxx.xom 
    i:/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=fordesign/ 
    Email=fordeisgn@21cn.com 
    ----BEGIN CERTIFICATE----- 
    MIIDdzCCAuCgAwIBAgIBATANBgkqhkiG9w0BAQQFADB8MQswCQYDVQQGEwJBVTET 
    MBEGA1UECBMKU29tZS1TdGF0ZTEhMB8GA1UEChMYSW50ZXJuZXQgV2lkZ2l0cyBQ 
    dHkgTHRkMRIwEAYDVQQDEwlmb3JkZXNpZ24xITAfBgkqhkiG9w0BCQEWEmZvcmRl 
    aXNnbkAyMWNuLmNvbTAeFw0wMDExMTIwNjE5MDNaFw0wMTExMTIwNjE5MDNaMH0x 
    CzAJBgNVBAYTAkFVMRMwEQYDVQQIEwpTb21lLVN0YXRlMQswCQYDVQQHEwJnejEP 
    MA0GA1UEChMGYWkgbHRkMQswCQYDVQQLEwJzdzESMBAGA1UEAxMJZm9yZGVzaWdu 
    MRowGAYJKoZIhvcNAQkBFgt4eHhAeHh4LnhvbTCBnzANBgkqhkiG9w0BAQEFAAOB 
    jQAwgYkCgYEAuQVRVaCyF+a8/927cA9CjlrSEGOL17+Fk1U6rqZ8fJ6UR+kvhUUk 
    fgyMmzrw4bhnZlk2NV5afZEhiiNdRri9f8loklGRXRkDfmhyUWtjiFWUDtzkuQoT 
    6jhWfoqGNCKh/92cjq2wicJpp40wZGlfwTwSnmjN9/eNVwEoXigSy5ECAwEAAaOC 
    AQYwggECMAkGA1UdEwQCMAAwLAYJYIZIAYb4QgENBB8WHU9wZW5TU0wgR2VuZXJh 
    dGVkIENlcnRpZmljYXRlMB0GA1UdDgQWBBS+WovE66PrvCAtojYMV5pEUYZtjzCB 
    pwYDVR0jBIGfMIGcgBRpQYdVvVKZ0PXsEX8KAVNYTgt896GBgKR+MHwxCzAJBgNV 
    BAYTAkFVMRMwEQYDVQQIEwpTb21lLVN0YXRlMSEwHwYDVQQKExhJbnRlcm5ldCBX 
    aWRnaXRzIFB0eSBMdGQxEjAQBgNVBAMTCWZvcmRlc2lnbjEhMB8GCSqGSIb3DQEJ 
    ARYSZm9yZGVpc2duQDIxY24uY29tggEAMA0GCSqGSIb3DQEBBAUAA4GBADDOp/O/ 
    o3mBZV4vc3mm2C6CcnB7rRSYEoGm6T6OZsi8mxyF5w1NOK5oI5fJU8xcf8aYFVoi 
    0i4LlsiQw+EwpnjUXfUBxp/g4Cazlv57mSS6h1t4a/BPOIwzcZGpo/R3g/fOPwsF 
    F/2RC++81s6k78iezFrTs9vnsm/G4vRjngLI 
    -----END CERTIFICATE----- 
    --- 
    Server certificate 
    subject=/C=AU/ST=Some-State/L=gz/O=ai ltd/OU=sw/CN=fordesign/ 
    Email=xxx@xxx.xom 
    issuer=/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=fordesign/ 
    Email=fordeisgn@21cn.com 
    --- 
    No client certificate CA names sent 
    --- 
    SSL handshake has read 1069 bytes and written 342 bytes 
    --- 
    New, TLSv1/SSLv3, Cipher is DES-CBC3-SHA 
    Server public key is 1024 bit 
    SSL-Session: 
    Protocol : SSLv3 
    Cipher : DES-CBC3-SHA 
    Session-ID: E1EC3B051F5DB8E2E3D3CD10E4C0412501DDD6641ACA932B65 
    DC25DCD0A3A86E 
    Session-ID-ctx: 
    Master-Key: 47DB3A86375DB2E99982AFD8F5B382B4316385694B01B74BFC3 
    FA26C7DBD489CABE0EE1B20CE8E95E4ABF930099084B0 
    Key-Arg : None 
    Start Time: 974010506 
    Timeout : 300 (sec) 
    Verify return code: 0 (ok) 
    --- 
    +OK AIMC POP service (sol7.gzai.com) is ready. 
    user ssltest0 
    +OK Please enter password for user <ssltest0>;. 
    pass ssltest0 
    +OK ssltest0 has 12 message (282948 octets) 
    list 
    +OK 12 messages (282948 octets) 
    1 21230 
    2 21230 
    3 21230 
    4 21230 
    5 21229 
    6 21230 
    7 21230 
    8 21230 
    9 111511 
    10 136 
    11 141 
    12 1321 
     . 
    quit 
    +OK Pop server at (sol7.gzai.com) signing off. 
    read:errno=0 
    SSL3 alert write:warning:close notify 
    --- 
    Certificate chain 
    0 s:/C=AU/ST=Some-State/L=gz/O=ai ltd/OU=sw/CN=fordesign/ 
    Email=xxx@xxx.xom 
    i:/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=fordesign/ 
    Email=fordeisgn@21cn.com 
    -----BEGIN CERTIFICATE----- 
    MIIDdzCCAuCgAwIBAgIBATANBgkqhkiG9w0BAQQFADB8MQswCQYDVQQGEwJBVTET 
    MBEGA1UECBMKU29tZS1TdGF0ZTEhMB8GA1UEChMYSW50ZXJuZXQgV2lkZ2l0cyBQ 
    dHkgTHRkMRIwEAYDVQQDEwlmb3JkZXNpZ24xITAfBgkqhkiG9w0BCQEWEmZvcmRl 
    aXNnbkAyMWNuLmNvbTAeFw0wMDExMTIwNjE5MDNaFw0wMTExMTIwNjE5MDNaMH0x 
    CzAJBgNVBAYTAkFVMRMwEQYDVQQIEwpTb21lLVN0YXRlMQswCQYDVQQHEwJnejEP 
    MA0GA1UEChMGYWkgbHRkMQswCQYDVQQLEwJzdzESMBAGA1UEAxMJZm9yZGVzaWdu 
    MRowGAYJKoZIhvcNAQkBFgt4eHhAeHh4LnhvbTCBnzANBgkqhkiG9w0BAQEFAAOB 
    jQAwgYkCgYEAuQVRVaCyF+a8/927cA9CjlrSEGOL17+Fk1U6rqZ8fJ6UR+kvhUUk 
    fgyMmzrw4bhnZlk2NV5afZEhiiNdRri9f8loklGRXRkDfmhyUWtjiFWUDtzkuQoT 
    6jhWfoqGNCKh/92cjq2wicJpp40wZGlfwTwSnmjN9/eNVwEoXigSy5ECAwEAAaOC 
    AQYwggECMAkGA1UdEwQCMAAwLAYJYIZIAYb4QgENBB8WHU9wZW5TU0wgR2VuZXJh 
    dGVkIENlcnRpZmljYXRlMB0GA1UdDgQWBBS+WovE66PrvCAtojYMV5pEUYZtjzCB 
    pwYDVR0jBIGfMIGcgBRpQYdVvVKZ0PXsEX8KAVNYTgt896GBgKR+MHwxCzAJBgNV 
    BAYTAkFVMRMwEQYDVQQIEwpTb21lLVN0YXRlMSEwHwYDVQQKExhJbnRlcm5ldCBX 
    aWRnaXRzIFB0eSBMdGQxEjAQBgNVBAMTCWZvcmRlc2lnbjEhMB8GCSqGSIb3DQEJ 
    ARYSZm9yZGVpc2duQDIxY24uY29tggEAMA0GCSqGSIb3DQEBBAUAA4GBADDOp/O/ 
    o3mBZV4vc3mm2C6CcnB7rRSYEoGm6T6OZsi8mxyF5w1NOK5oI5fJU8xcf8aYFVoi 
    0i4LlsiQw+EwpnjUXfUBxp/g4Cazlv57mSS6h1t4a/BPOIwzcZGpo/R3g/fOPwsF 
    F/2RC++81s6k78iezFrTs9vnsm/G4vRjngLI 
    -----END CERTIFICATE----- 
    --- 
    Server certificate 
    subject=/C=AU/ST=Some-State/L=gz/O=ai ltd/OU=sw/CN=fordesign/ 
    Email=xxx@xxx.xom 
    issuer=/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=fordesign/ 
    Email=fordeisgn@21cn.com 
    --- 
    No client certificate CA names sent 
    --- 
    SSL handshake has read 1579 bytes and written 535 bytes 
    --- 
    New, TLSv1/SSLv3, Cipher is DES-CBC3-SHA 
    Server public key is 1024 bit 
    SSL-Session: 
    Protocol : SSLv3 
    Cipher : DES-CBC3-SHA 
    Session-ID: E1EC3B051F5DB8E2E3D3CD10E4C0412501DDD6641ACA932B65DC2 
    5DCD0A3A86E 
    Session-ID-ctx: 
    Master-Key: 47DB3A86375DB2E99982AFD8F5B382B4316385694B01B74BFC3FA 
    26C7DBD489CABE0EE1B20CE8E95E4ABF930099084B0 
    Key-Arg : None 
    Start Time: 974010506 
    Timeout : 300 (sec) 
    Verify return code: 0 (ok)



