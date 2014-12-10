autovpn
=======
openwrt pptp vpn setup
-------
在路由器上做vpn是最方便的无限制浏览网站的方法 大部分都是参考了audoddvpn,如果是ddwrt可以直接参考它的方法<br/>
如果是openwrt 可以参考 http://code.google.com/p/openwrt-smarthosts-autoddvpn/ 
和http://www.cnblogs.com/fatlyz/p/3843581.html 及 https://github.com/SteamedFish/gfwiplist 
来进行路由器vpn设置。

 
####1.pptp模块安装
###
    opkg update
    opkg install ppp-mod-pptp
####2.pptp设置
  在luci界面填入pptp服务器及账户信息 
  名称为了后文方便使用GFWVPN 
  在高级设置界面最好不要选中默认路由
  在firewall界面 选择wan接口
  pptp如果连不上 主要是因为加密参数的问题 可以 logread看相应的日志
  如果连不上 编辑  /etc/ppp/options.pptp  
  根据实际情况加入加入(不需要全部加入) 
###  
    mppe required
    mppe stateless
    refuse-pap
    refuse-chap
    refuse-eap
    refuse-mschap
建立 /etc/ppp/ip-up.d 文件夹 和 /etc/ppp/ip-up.d/pptp-GFWVPN文件
别忘了添加权限 
###
    chmod a+x /etc/ppp/ip-up.d/pptp-GFWVPN 
    chmod a+x /etc/ppp/gen-route.sh 

####3.dnsmasq设置
dnsmasq设置就比较简单了 直接把相应的域名按照
###
    server=/google.com/8.8.4.4
的格式加入即可
####4.国外路由表设置
国外路由表格式 
###
    route add -host 8.8.8.8 dev $1
    route add -host 8.8.4.4 dev $1
    route add -host 208.67.222.222 dev $1
    route add -net 174.36.30.0/24 dev $1
  
至此及大功告成 windows下如果还是不能访问 可以 ipconfig /flushdns 清理下dns缓存  
调试的时候可以使用 traceroute命令跟踪。<br>
ps在实际使用的过程中,发现 route add 添加时有条数限制。
当路由表添加条数过多时后添加的不会生效 不知道是rom的问题还是oepnwrt本身的问题
所以提供的路由表只包含google facebook和 twitter 可以根据实际情况添加


