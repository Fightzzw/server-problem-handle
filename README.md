# 服务器网络问题与解决
1.Xshell和mobaxterm能ping通但无法远程连接服务器
 
解决方法：禁用以太网的网卡
 
2.在Linux 服务器上执行.sh文件提示：permission denied.
 
原因：因为当前目录下没有执行.sh文件的权限,所以导致无法执行该脚本;在该目录下执行:chmod u+x *.sh再次去执行你的命令就OK了
解决方法：在该目录下执行:chmod u+x *.sh
 

3.Infiniswap构建过程问题
 
出现问题：
 
 
解决：
1）	把ib0改为eno3

 
4.160服务器突然出现network error，一直无法连接
Session stopped
    - Press <return> to exit tab
    - Press R to restart session
    - Press S to save terminal output to file

Network error: Connection timed out
且无法ping通
 
问题：在问题3设置InfiniBand NIC时，把服务器网关DNS给整没了，需要重启网络服务
解决：登录BMC,去重启网络服务
登录方式：
1在浏览器输入BMC的IP，160的为191.4.50.158，161的为159就可进入，但注意浏览器会提示网址有风险，证书错误，不用管，点详细信息进去。
2使用软件KVM（KVMCLIENT）
 
介绍1：基板管理控制器（Baseboard Manager Controller，简称BMC）。
在介绍BMC之前需要了解一个概念，即平台管理（platform management）
平台管理表示的是一系列的监视和控制功能，操作的对象是系统硬件。比如通过监视系统的温度，电压，风扇、电源等等，并做相应的调节工作，以保证系统处于健康的状态。
需要说明的是，BMC是一个独立的系统，它不依赖与系统上的其它硬件（比如CPU、内存等），也不依赖与BIOS、OS等（但是BMC可以与BIOS和OS交互，这样可以起到更好的平台管理作用，OS下有系统管理软件可以与BMC协同工作以达到更好的管理效果）。

一般我们的电脑不会带BMC，因为用处不大，一些温度、电源等的管理，CPU来控制就够了。
————————————————
版权声明：本文为CSDN博主「BaldStrong」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_42306122/java/article/details/95642613
介绍2：
BMC是一个独立于服务器系统的小型操作系统，作用是方便服务器远程管理、监控、安装、重启等操作。
BMC接通电源即启动运行，由于独立于业务程序不受影响，避免了因死机或者重新安装系统而进入机房。BMC只是一个集成在主板上的芯片（也有通过PCIE等各种形式插在主板上），对外表现形式只有一个标准RJ45网口，拥有独立IP。普通维护只需使用浏览器访问IP:PORT登录管理页面，服务器集群一般使用BMC指令进行大规模无人值守操作。
一般服务器BMC网口是独立的，仔细看印有BMC字样。但是也有小型服务器BMC网口和通信网口是二合一的。
当然也有不叫BMC的，只要遵守IPMI协议，都是类似的。
具体解决方法
（1）
 
用户名：Administrator
密码：Admin@9000
（2）
 
（3）
1直接解决法：进入root模式，输入命令reboot（重启服务器即可）
2重启网络服务命令:（16.04版亲测有效）
14.04版参看网址:(亲测重启网络服务无法解决)
https://www.cnblogs.com/sea-stream/p/11053160.html
route –n
发现没有网关
 
然后重启网络服务
 
就此完成！！！！！
 

5.安装时错误，Permission denied
./install.sh: line 97: ./autogen.sh: Permission denied
./install.sh: line 98: ./configure: No such file or directory
make: *** No targets specified and no makefile found.  Stop.
make: *** No rule to make target 'install'.  Stop.
进行到这一步
 
问题：
 
解决：infiniswap_bd目录下的.sh文件没有权限，参考问题2
 
6.安装infiniswap_bd时大量error
 
原因：Linux版本，要求Ubuntu 14.04 (kernel 3.13.0, also tested on 4.4.0/4.11.0)，而我们的是Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-112-generic x86_64)。
其中bio的结构定义不一样
参考文章：https://my.oschina.net/hand/blog/2244957
解决重装系统。
7.重装系统
 
Passward:Huawei@123
8．操作服务器三种方式
1.直接到机房，服务器连接键盘鼠标，跟操作电脑一样。
2.用软件kvm进行BMC登录，进入远程桌面
3.用xshell或者MobaXterm使用ssh服务（一种加密协议，在服务器端有公钥和对应私钥，用户请求登录时服务器发公钥给用户，用户用公钥加密密码发给服务器，服务器用私钥解密；然而此过程若有人伪装服务器给用户发公钥，则用户密码泄露）远程登录服务器并操作，不同于2的远程桌面。（前提是服务器要下载ssh-server,例如openssh-server，并打开ssh服务）

9.linux自带python为2.7，如何安装新的python3.8.5，并使python默认指向python3.8.5
解决：参看这两篇博文
https://blog.csdn.net/qq_15260769/article/details/88822620?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight

https://blog.csdn.net/u014775723/article/details/85213793?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight

解决：这样后来会发现pip不能用因为ssl出了问题，之后会更新openssl并到Python-3.8.5目录下去修改Modules/Setup这个配置文件，然后重新配置编译 ，参见问题10，
1、	下载python3.8.5
wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
2、	解压
tar zxvf Python-3.8.5.tgz
cd Python-3.8.5
3、	编译
./configure –with-openssl
或者保险：./configure –withopenssl=/usr/local/openssl
make 
sudo make install
4、	查看python3.8.5位置
which python3.8（这个只会把可执行的python3.8位置列出来，这正是我们想要的）
或者：whereis python3.8(这个命令会把包含python3字符的所有文件位置列出来)
5、	添加软连接
先备份原来的，再新建，参考这个
1.	mv /usr/bin/python /usr/bin/python.bak
2.	ln -s /usr/local/python3.7.1/bin/python3.7 /usr/bin/python
3.	mv /usr/bin/pip /usr/bin/pip.bak
4.	ln -s /usr/local/python3.7.1/bin/pip3 /usr/bin/pip
6、	输入python验证
 
okk了！！！！！
4.1浏览器进不了服务器BMC
 
解决：服务器的BMC网址不能用代理，所以要在不使用代理服务器的网段加上”*.huawei.com;10.*”，为确保万无一失，我还添加了“191.*”.
 
 
 
 
然后就OK了
 
10．Python3.8.5使用pip时出现：Can't connect to HTTPS URL because the SSL module is not available. - skipping

原因：linux系统原装的ssl服务版本较低为1.0.几，而python要求的OpenSSL要1.1以上。所以要重新安装OpenSSL。

OpenSSL介绍：在计算机网络上，OpenSSL是一个开放源代码的软件库包，应用程序可以使用这个包来进行安全通信，避免窃听，同时确认另一端连接者的身份。这个包广泛被应用在互联网的网页服务器上。
SSL是Secure Sockets Layer（安全套接层协议）的缩写，可以在Internet上提供秘密性传输。Netscape公司在推出第一个Web浏览器的同时，提出了SSL协议标准。其目标是保证两个应用间通信的保密性和可靠性,可在服务器端和用户端同时实现支持。已经成为Internet上保密通的工业标准。
SSL能使用户/服务器应用之间的通信不被攻击者窃听，并且始终对服务器进行认证，还可选择对用户进行认证。SSL协议要求建立在可靠的传输层协议(TCP)之上。SSL协议的优势在于它是与应用层协议独立无关的，高层的应用层协议(例如：HTTP，FTP，TELNET等)能透明地建立于SSL协议之上。SSL协议在应用层协议通信之前就已经完成加密算法、通信密钥的协商及服务器认证工作。在此之后应用层协议所传送的数据都会被加密，从而保证通信的私密性。

解决：参看这篇博文
https://www.cnblogs.com/muxizZ/p/13408895.html

11.服务器改成允许root登录
 

 
12.修改slaves重建集群
1、stop集群
2、删hdfs里data, name里的东西
3、格式化
4、start集群
13.无法建集群，12方法无效
1，修改了hibench.conf 删除了slaves里的主节点

14.30G数据hadoop跑到reduce55%会卡;spark会报错，slave lost
由于160未开启daemon程序完成时间和开启差不太多，我怀疑未使用远程内存，所以决定将数据量从25G增为30G，然后遇到问题
hadoop
 
spark
 

重新建立集群后，仍然跑不完
30G数据：修改内存为20G，实际为17G后，spark仍然报错，hadoo跑到reduce65%也跑不动.27G数据Hadoop跑到reduce86%停住
资料显示，Hadoop处理大量小文件的熟读慢于处理同等大小的大文件的速度。

当卡死后再次准备数据或者run,无法启动，无响应，卡在mapreducejob部分。

15．将install.sh关于内存的设置全部改为默认后，有内存的调用了
 
16，重启服务器后guest比root在shell中登录的更快
17．161内存修改为8G无法直接重启，强制下电再上电才可重启。
2020.08.17
19.161内存修改为10G(7.4G)，开启bd并与daemon取得通信调用内存后，老是卡住。只能重启
 
2020.08.17重启后再次配置，终于跑成功一次。

20.Linux永久添加静态路由（14.04,16.04亲测有效）

网址：https://blog.csdn.net/qq_33876106/article/details/90236094
1.	方法三：修改 interface 配置文件
范围：测试 debian 8.1、Ubuntu 14.04 系统可用
使用方法：
2.	向系统的 /etc/network/interface 文件中添加命令 up route add -net 192.168.2.0/24 gw 192.168.2.254(服务器的默认路由为192.168.2.254)，或者添加命令 up route add -net 192.168.3.0/24 dev eth0，前者是依赖默认路由进行数据的转发，后者是依赖网卡接口进行数据的转发
3.	reboot 重启系统命令生效
4.	用 route -n 命令查看命令是否已经生效
 
21．BMC网关配错，导致无法登录BMC解决办法
配错BMC网关的服务器为M1
条件：
1.	如果在M1 的IP正确，
2.	且在该IP同一子网下有另一台网关配置正确
解决：
1可以登录BMC的服务器M2，那么可以登录M2的BMC，
2然后在M2的远程桌面打开浏览器，在浏览器地址栏输入M1的BMC IP可以登录到M1的BMC远程桌面，
3然后再把M1的网关重新配置正确。
4，这样就可以在本地用KV或者浏览器访问M1的BMC了
原理：因为M1和M2在同一网段（子网）下，所以M1与M2的通信不需要通过网关，故M1网关设置错了也可以由M2访问，但是前提是你要在本地主机能访问M2的BMC，这就要求M2的网关设置正确。
注意：如果是有界面的linux服务器，则直接点击firefox图标，就可以直接打开浏览器。
如果是无界面的Linux服务器，不慌，一般情况下，在命令行中输入firefox ，就会在本地主机弹出Firefox的界面。因为服务器安装了Xmanager，这样在Linux命令行中输入有图形界面的应用时，服务器可以自动弹出图形界面。
Xmanager:https://blog.csdn.net/linghao00/article/details/8768435
