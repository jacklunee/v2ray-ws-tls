#
注意：务必保证域名解析已经成功了，再使用下面的脚本安装。

打开电脑命令行，ping 你的域名，如果显示VPS的IP地址，则解析生效了。

如果你要用cloudflare等CDN并且你已经切换到cloudflare的DNS，那么安装时注意不要开启CDN（云朵不要点亮，要是灰色的），安装完成可以开启（开启是走CDN，不开启直连VPS）。

1、使用SSH工具连接VPS，执行下列命令，选择安装v2ray+ws+tls

curl -O https://raw.githubusercontent.com/atrandys/v2ray-ws-tls/master/v2ray_ws_tls1.3.sh && chmod +x v2ray_ws_tls1.3.sh && ./v2ray_ws_tls1.3.sh
2、等待脚本执行，过程中会提示需要输入域名，输入解析到本VPS的域名，然后回车

3、等待安装完成，你可以看到配置参数，客户端配置时用到。

4、安装BBR加速，指向下面命令

cd /usr/src && wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
5、注意在弹出的安装界面首先选择1，安装BBR内核,安装过程可能时间较长,耐心等待。

6、安装完成后会提示重启VPS,输入Y，然后回车，确认重启。然后等待几分钟，再使用xshell连接vps（连接方法是点软件上打开，找到之前保存的连接，然后点连接）登陆后执行下列命令

cd /usr/src && ./tcp.sh
7、在弹出安装界面,输入5,然后回车，使用BBR魔改版加速，等待安装完成提示bbr启动成功即可。

8、注意多次重复安装可能会使域名申请证书出错，不要出错误一直重试。

客户端配置
1、下载v2ray客户端

v2ray各平台客户端：https://www.v2ray.com/awesome/tools.html

2、将参数对应填写到客户端

这里大概说明一下参数怎么填写：

地址：你的域名，例如google.com

端口：443

用户ID：就是一长串uuid

加密方式：aes-128-gcm

传输协议：ws

path：就填路径这个参数

底层传输：tls

3、关于移动端说明

目前有小伙伴反映，这个方案下，有的客户端可用有的不可用，那么需要你在保证配置正确的情况下，多试几个客户端（有的手机客户端/路由器客户端不支持tls1.3，所以用不了）。

个人现在主要用justmysocks，开头推荐的那个瓦工机场，主要是省心，所以关于这个方案的移动客户端使用情况，我给不了什么参考意见。

伪装网站配置
脚本已经为你创建了一个英文模板站，如果你想自己为VPS配置一个其他的伪装站点，可自行获取网页文件，入口页包含index.html，将其传输到VPS的/etc/nginx/html目录下，重启VPS即可。

如何套cloudflare CDN
套cloudflare CDN需要再以上教程搭建完成的基础上进行。

1、申请cloudflare账号，这个自己去搞定

2、添加网站，输入你的域名，然后添加解析（注意要开启cdn），根据提示，将域名的nameserver改为cloudflare提供的值。

3、等待cloudflare生效，一般很快就OK了。v2ray的参数不需要任何修改。
