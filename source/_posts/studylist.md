---
title: 零散笔记
tags: [学习笔记,IT]
categories: [其他] 
date: 2016-09-20 10:13:41
---

<div>

# 2016.09.20之前笔记
## 学习笔记##

**7.10** **sass和compass**:Sass是一个CSS3的扩展语言，它提供了丰富的特性使得编写样式更加容易：嵌套样式，变量定义，扩展，mixin等等;Compass是一个使用了Sass的库，它将很多常用样式打包成了一些模块，我们可以在自己的项目中使用这些模块，比如模块reset可以用来将不同浏览器的差异抹平,css3则用来生成CSS3相关的属性等$ gem install compass$ compass create projectNamesublime插件:emmet,html-css-js
**7.11**ubuntu16.04安装网易云音乐 源可换成阿里源；安装shadowsocks客户端时，linux使其自启动可在/etc/rc.local中 exit0 前加入命令 sudo ssolocal -c /etc/shadowsock.json 不需要在后面加-d start
**7.12**mongodb需要安装mongdb-server和mongodb-clients才可以不用一直输入./mongod --dbpath ../blog/打开mongodb等待连接mongodb删除数据库：db.dropDatabase()；删除集合：db.collection.drop()<!--more-->
**7.26**今天复习了下bootstrap，这几天都有点颓废，把亮剑小说看完了，瞬间热血沸腾，其实电视里拍的只是小说的一部分啊，我想我应该学习老李的是勇敢顽强，为了真理永远不能屈服，那么才能培养出一生正气至于文革，我想说的是很同意知乎上一个知友的说法，对于日本篡改教科书，不承认南京大屠杀，当局强烈谴责；那么对于自己的wg,64,为什么不能正确面对；我希望未来的中国是民主的正气的自由的积极向上的，我想中华民主几千年文明的底蕴传承，中国未来是有希望的。而我也要在其中做出自己的一份贡献，最近看了很多不是那么美好的事实，无知是幸福的，但是真理是我们应该追求的，即使痛苦。但是庆幸的是，我还是个积极向上的阳光青年值得注意的是别那么傲娇了，嘴下积德，好好的读好书，好好学习本领（听着 《再见 小时候》好听）
&amp;nbsp 表示Non-breaking spacebootstrap加element不要直接在元素里加row class
**8.02**JS对象的属性用.（点）叠加，数组用 [下标] 来访问。
**8.04**JS中str是不变的，str.toLowerCase()/str.toUpperCase()不能改变原字符串
**8.14**JS中break continue可以加标签，break ：label跳转到该标签位置
**8.17**debian开机自启动，网上查看开机自启动脚本位于/etc/local，但是假如脚本无法启动，也没有/etc/rc.d/rc.local这个文件，通过vim /etc/inittab查看需要启动服务的script的放置路径，我的vps位于/etc/init.d/rc.local，直接在这个里添加脚本可以开机自启动
**8.28**ll命令：打开.bashrc文件修改ll，并使其生效 source .bashrc
**8.31**rtl8188eus无线网卡内核驱动，(来自：https://github.com/lwfinger/)
rtl8188eu通过此处代码编译，编译流程：configure,make all,make install双系统linux系统时间要比windows快8小时，Windows/Ubuntu 双系统用户会发现在 Ubuntu 里面的时间正常的情况下Windows的系统时间被改到8小时前。 原来 Linux 操作系统是以 CMOS 时间做为格林威治标准时间，再根据系统设置的时区来确定目前系统时间。但是Windows 会直接修改CMOS 时间。而中国的时区是+8区，所以才会造成时间被调整了-8个小时。所以您可以让 Windows 去使用时区或者让 Ubuntu 使用本地时间。修改 Windows 使用时区的方法是在注册表： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\ 下面增加一个名为 RealTimeIsUniversal 的REG_DWORD 键，并赋值为 1。而让Ubuntu 使用本地时间的方法是： sudo gedit /etc/default/rcS 把里面的 UTC=yes 改为 UTC=no (来自：http://blog.chinaunix.net/uid-8305736-id-2033022.html)
**互联网网络协议：**

- 实体层：把电脑连接起来的物理手段。它主要规定了网络的一些电气特性，作用是负责传送0和1的电信号。

- 链接层：确定了0和1的分组方式。以太网协议：一帧(frame)：Head+Data;Head包含发送和接受者的信息，即网卡地址（MAC地址），每个网卡的地址是独一无二的；广播：(arp协议识别接收方网卡地址)以太网它不是把数据包准确送到接收方，而是向本网络内所有计算机发送，让同一子网每台计算机自己判断MAC地址与标头中接受方的MAC地址，是否为接收方。（数据包的定义、网卡的MAC地址、广播的发送方式）

- 网络层：
1.  必须找到一种方法，能够区分哪些MAC地址属于同一个子网络，哪些不是。如果是同一个子网络，就采用广播方式发送，否则就采用"路由"方式发送。（"路由"的意思，就是指如何向不同的子网络分发数据包，这是一个很大的主题）；这就导致了"网络层"的诞生，它的作用是引进一套新的地址，使得我们能够区分不同的计算机是否属于同一个子网络。这套地址就叫做"网络地址"，简称"网址"。"网络层"出现以后，每台计算机有了两种地址，一种是MAC地址，另一种是网络地址。两种地址之间没有任何联系，MAC地址是绑定在网卡上的，网络地址则是管理员分配的，它们只是随机组合在一起。网络地址帮助我们确定计算机所在的子网络，MAC地址则将数据包送到该子网络中的目标网卡。因此，从逻辑上可以推断，必定是先处理网络地址，然后再处理MAC地址。
2.  规定网络地址的协议叫IP协议，IP4为32地址，前24位为网络部分，后8位为主机部分，处于同一子网络的计算机的网络部分应相同。所谓"子网掩码"，就是表示子网络特征的一个参数。它在形式上等同于IP地址，也是一个32位二进制数字，它的网络部分全部为1，主机部分全部为0。判断方法是将两个IP地址与子网掩码分别进行AND运算（两个数位都为1，运算结果为1，否则为0），然后比较结果是否相同，如果是的话，就表明它们在同一个子网络中，否则就不是。IP数据包直接放进以太网中的数据部分。不在同一个子网络中需要通过网关转发，而使用ARP协议可以得到同一个子网络内的主机MAC地址。

- 传输层：我们还需要一个参数，表示这个数据包到底供哪个程序（进程）使用。这个参数就叫做"端口"（port），它其实是每一个使用网卡的程序的编号。每个数据包都发到主机的特定端口，所以不同的程序就能取到自己所需要的数据。。"传输层"的功能，就是建立"端口到端口"的通信。相比之下，"网络层"的功能是建立"主机到主机"的通信。只要确定主机和端口，我们就能实现程序之间的交流。因此，Unix系统就把主机+端口，叫做"套接字"（socket）。UDP协议：UDP数据包，也是由"标头"和"数据"两部分组成。"标头"部分主要定义了发出端口和接收端口，"数据"部分就是具体的内容。然后，把整个UDP数据包放入IP数据包的"数据"部分，而前面说过，IP数据包又是放在以太网数据包之中的。特点是简单，但可靠性差。TCP协议：可以近似认为，它就是有确认机制的UDP协议，每发出一个数据包都要求确认。如果有一个数据包遗失，就收不到确认，发出方就知道有必要重发这个数据包了。TCP协议能够确保数据不会遗失。它的缺点是过程复杂、实现困难、消耗较多的资源。TCP数据包和UDP数据包一样，都是内嵌在IP数据包的"数据"部分。TCP数据包没有长度限制，理论上可以无限长，但是为了保证网络的效率，通常TCP数据包的长度不会超过IP数据包的长度，以确保单个TCP数据包不必再分割。（以太网数据包的数据部分，最大长度为1500字节，超出需分割）

- 应用层："应用层"的作用，就是规定应用程序的数据格式。举例来说，TCP协议可以为各种各样的程序传递数据，比如Email、WWW、FTP等等。那么，必须有不同协议规定电子邮件、网页、FTP数据的格式，这些应用程序协议就构成了"应用层"。这是最高的一层，直接面对用户。它的数据就放在TCP数据包的"数据"部分。因此，现在的以太网的数据包就变成下面这样。

**上网设置：**
- 本机的IP地址
- 子网掩码
- 网关的IP地址
- DNS的IP地址（把网址转换为IP地址）

**9.5** shadowsocks,500 internal privoxy error.怎么解决？（https://www.zhihu.com/question/40771057）
以管理员身份运行命令提示符（cmd），执行： netsh interface ipv4 reset netsh interface ipv6 reset netsh winsock reset 重启电脑，解决。
**9.10** Vim 编辑文件报：Swap file "Hello.java.swp" already exists!问题原因： Vim 编辑 Hello.java 文件的时候，非正常退出，然后又重新再 Vim 这个文件一般都会提示这个。解决办法： 进入被编辑的文件目录，比如：Hello.java 我放在 /opt 目录下，那就先：cd /opt， 然后：ls -A，会发现有一个：.Hello.java.swp，把这个文件删除掉：rm -rf .Hello.java.swp，然后重新 Vim 文件即可。
**9.20** wordpress安装：LAMP；数据库建立；5分钟安装；插件授权录后，执行`sudo chown -R www-data /var/www/wordpress
sudo chmod -R 775 /var/www/wordpress `
这样就完全解决问题了.注意了,不需要把/var/www/目录的所有者也设置为www-data,而只需要设置wordpress文件夹的所有者.wp-config.php里加入下面代码:` define("FS_METHOD","direct"); define("FS_CHMOD_DIR",0777); define("FS_CHMOD_FILE",0777);`
</div>
&nbsp;

__________________
2017-08-27更新
__________________
# 2017-08-27之后笔记
## unbuntu引导设置默认
由于双系统问题，每次启动都需要手动选择启动，查阅资料发现可以对其引导项默认值进行修改，`sudo vim /etc/default/grub`打开grub文件进行修改，`GRUB_DEFAULT=0`修改为想要默认启动的序号，序号默认从0开始，我的windows为第五项序号为4改为`GRUB_DEFAULT=4`

## vps中的shadowsocks版本更换
由于之前python版本的shadowsocks经常自动关闭，而且现在更新也不够及时，选择shadowsocks-libev
[参见 安装shadowsocks-libev](http://happylg.cn/2017/08/27/right-to-Internet/#安装)