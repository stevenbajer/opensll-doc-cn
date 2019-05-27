openssl简介－入门
实现了SSL的软件不多，但都蛮优秀的。首先，netscape自己提出来的概念，当然自己会实现一套了。netscape的技术蛮优秀的，不过我没用过他们的ssl-toolkit.甚至连名字都没搞清楚。 
    1995年，eric.young开始开发openssl, 那时候叫ssleay.一直到现在，openssl还在不停的修改和新版本的发行之中。openssl真够大的，我真佩服eric的水平和兴趣。这些open/free的斗士的精神是我写这个系列的主要动力，虽然写的挺烦的。
ps: eric现在去了RSA公司做，做了一个叫SSL-C的toolkit, 其实和openssl差不多。估计应该比openssl稳定，区别是这个是要银子的，而且几乎所有低层的函数都不提供直接调用了。那多没意思。 
    去www.openssl.org down openssl吧，最新的是0.9.6版。 
    安装是很简单的。我一直用的是sun sparc的机器，所以用sun sparc的机器做例子。 
    gunzip -d openssl.0.9.6.tar.gz 
    tar -xf openssl.0.9.6.tar 
    mv openssl.0.9.6 openssl 
    cd openssl 
    ./configure --prefix=XXXXX --openssldir=XXXXXXXX 
    (这里prefix是你想安装openssl的地方， openssldir就是你tar开的openssl源码的地方。好象所有的出名点的free software都是这个操行，configure, make , make test, make install, 搞定。) 
    ./make(如果机器慢，这一步的时候可以去洗个澡，换套衣服) 
    ./make test 
    ./make install 
    OK, 如果路上没有什么问题的话，搞定。 
    经常有人报bug, 在hp-ux, sgi上装openssl出问题，我没试过，没发言权。 
    现在可以开始玩openssl了。 
    注意： 我估计openssl最开始是在linux下开发的。大家可以看一看在linxu下有这么一个文件：/dev/urandom, 在sparc下没有。这个文件有什么用？你可以随时找它要一个随机数。在加密算法产生key的时候，我们需要一颗种子：seed。这个seed就是找/dev/urandom要的那个随机数。那么在sparc下，由于没有这么一个设备，很多openssl的函数会报错："PRNG not seeded". 解决方法是：在你的~/.profile里面添加一个变量$RANDFILE， 设置如下： 
    $RANDFILE=$HOME/.rnd 
    然后在$HOME下vi .rnd, 随便往里面乱输入一些字符，起码俩行。 
    很多openssl的函数都会把这个文件当seed, 除了openssl rsa, 希望openssl尽快修改这个bug. 
    如果用openssl做toolkit编程， 则有其他不太安全的解决方法。以后讲到openssl编程的章节会详细介绍。 
    先生成自己的私有密钥文件，比如叫server.key 
    openssl genrsa -des3 -out server.key 1024 
    genras表示生成RSA私有密钥文件，-des3表示用DES3加密该文件，1024是我们的key的长度。一般用512就可以了，784可用于商业行为了，1024可以用于军事用途了。 
    当然，这是基于现在的计算机的速度而言，可能没过几年1024是用于开发测试，2048用于一般用途了。 
    生成server.key的时候会要你输入一个密码，这个密钥用来保护你的server.key文件，这样即使人家偷走你的server.key文件，也打不开，拿不到你的私有密钥。 
    openssl rsa -noout -text -in server.key 
    可以用来看看这个key文件里面到底有些什么东西(不过还是看不懂) 
    如果你觉得server.key的保护密码太麻烦想去掉的话： 
    openssl rsa -in server.key -out server.key.unsecure 
    不过不推荐这么做 

下一步要得到证书了。得到证书之前我们要生成一个Certificate Signing Request. 
    CA只对CSR进行处理。 
    openssl req -new -key server.key -out server.csr 
    生成CSR的时候屏幕上将有提示,依照其指示一步一步输入要求的信息即可. 
    生成的csr文件交给CA签名后形成服务端自己的证书.怎么交给CA签名？ 
    自己去www.verisign.com慢慢看吧。 

如果是自己玩下，那么自己来做CA吧。openssl有很简单的方法做CA.但一般只好在开发的时候或者自己玩的时候用，真的做出产品，还是使用正规的CA签发给你的证书吧 
    在你make install之后，会发现有个misc的目录，进去，运行CA.sh -newca，他会找你要CA需要的一个CA自己的私有密钥密码文件。没有这个文件？按回车创建，输入密码来保护这个密码文件。之后会要你的一个公司信息来做CA.crt文件。最后在当前目录下多了一个./demoCA这样的目录../demoCA/private/cakey.pem就是CA的key文件啦， 
    ./demoCA/cacert.pem就是CA的crt文件了。把自己创建出来的server.crt文件copy到misc目录下，mv成newreq.pem,然后执行CA.sh -sign, ok, 
    得到回来的证书我们命名为server.crt. 

看看我们的证书里面有些什么吧 
    openssl x509 -noout -text -in server.crt 
    玩是玩过了，openssl的指令繁多，就象天上的星星。慢慢一个一个解释吧。

 