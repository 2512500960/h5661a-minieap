注意：本教程针对（极路由1s）h5661a，cpu是7628an（7629）的那个极路由1s。
	会导致无法保修，因为以下操作没有备份key。想要保修自行百度如何备份key

	文件描述：
minieap 认证客户端二进制文件，用PandoraBox-Toolchain-ralink-for-mipsel_24kec+dsp-gcc-4.8-linaro_uClibc-0.9.33.2.tar 这个toolchain编译的
PandoraBox-ralink-mt7628-mt7628an-evb-squashfs-sysupgrade-r1752-20151201 uboo,bootloader,换新固件前要刷写
u-boot-pandorabox-mt7628an-evb-20150518 pandorabox固件
hc5661a_eeprom.bin 如果出现无线不能使用需要到uboot的webrecovery界面刷写一次，建议第一次刷pandorabox的时候一并刷写了
PandoraBox-Toolchain-ralink-for-mipsel_24kec+dsp-gcc-4.8-linaro_uClibc-0.9.33.2.tar pandorabox的toolchain,如果要自己编译，在这里下载http://downloads.openwrt.org.cn/PandoraBox/PandoraBox-Toolchain-ralink-for-mipsel_24kec%2Bdsp-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2

	本教程操作包括：
准备工作：
	1、自己想办法而将极路由1s连上网，去http://192.168.199.1/下找云插件页面里面安装开发者APP，云平台--路由器信息--高级设置，获得root权限
	2、安装putty和winscp，在电脑上，等下要用。
	3、使用winscp登入你的极路由，账号：root、密码 ：你的后台密码 端口：22/1022 模式：SCP，登陆成功后进入/tmp目录，将breed-mt7620-hiwifi-hc5761.bin上传到这个目录
	4.使用putty登入你的极路由，账号密码端口与上述相同，登入成功后键入以下命令  mtd -r write /tmp/breed-mt7620-hiwifi-hc5761.bin u-boot
	注：如果上述步骤出错导致breed写入失败，或者干脆就是路由器cpu不是7628an的，需要用usb-ttl自己把breed（uboot）和eeprom刷回来。
开始刷pandorabox：
	PC用网线连路由器LAN，设置为自动获取IP。
	路由器断电，按住reset 加电（不松开reset）。
	保持按住reset 5秒左右，路由器灯闪。
	PC网卡获取到192.168.1.x的地址 （如未获取到手工设置）
	浏览器访问 192.168.1.1
	接着你就会看到一个uboot控制台的界面。
	刷入PandoraBox-ralink-mt7628-mt7628an-evb-squashfs-sysupgrade-r1752-20151201和hc5661a_eeprom.bin
	等待重启。
放入我编译好的minieap客户端二进制文件：
	上述pandorabox刷好之后。默认root密码是admin或者没设需要手动设置，登录管理后台192.168.1.1，修改wan口的参数，手动获取地址。需要mac地址克隆以实际情况而定。也可以去
微信网络中心服务平台解绑mac先。
	用winscp把minieap文件传入/tmp:地址192.168.1.1，端口号22，用户名root，密码默认admin，修改了的话填修改了的
	用putty连上路由器，地址192.168.1.1，端口号22，用户名root，密码默认admin，修改了的话填修改了的。接下来要用命令行操作，
	cp /tmp/minieap /usr/bin/
	chmod 777 /usr/bin/minieap
	touch /etc/minieap.conf
	ifconfig                    //这一步查看下之前的网络参数设置好了没。顺带看下WAN口的接口名。应该是eth0.2
	minieap -u 你的学号 -p 你的校园网密码（自己设置的那个，别总想不起来自己设置的密码就拿教务密码在上面蹭蹭蹭） -n eth0.2 --if-impl sockraw --module printer --module rjv3 --server default -b 2 -w
	认证成功的话就可以加入开机启动了
	加入开机启动 : 在/etc/rc.local 里面加入minieap一行，具体百度，vim不会用的可以用winscp编辑。
	
