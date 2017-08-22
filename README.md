# toutyrater.github.io



rm -rf /etc/localtime    #删除当前默认时区www.kwx.gd
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 
#复制替换默认时区为上海


对于 V2Ray，它的验证方式包含时间，就算是配置没有任何问题，如果时间不正确，也无法连接 V2Ray 服务器的
，服务器会认为你这是不合法的请求。所以系统时间一定要正确，只要保证时间误差在一分钟之内就没问题。


点这里https://github.com/v2ray/v2ray-core/releases下载 V2Ray 的 Windows 压缩包
，如果是 32 位系统，下载 v2ray-windows-32.zip
，如果是 64 位系统，下载 v2ray-windows-64.zip。
解压之后会有 v2ray.exe 和 config.json 这两个文件



但是现在还不能科学上网，因为 V2Ray 将所有选择权交给用户，它不会自动帮你设置系统代理
，因此还需要在浏览器里设置代理。以火狐（Firefox）为例，点菜单 -> 选项 -> 高级 ->
设置 -> 手动代理设置，在 SOCKS Host 填上 127.0.0.1，后面的 Port 填 1080，
再勾上使用 SOCKS v5 时代理 DNS (这个勾选项在旧的版本里叫做远程 DNS)。操作图见下：


在 Linux 操作系统， V2Ray 的安装有脚本安装、手动安装、编译安装 3 种方式，
选择其中一种即可，本指南仅提供使用使用脚本安装的方法，并仅推荐使用脚本安装，
该脚本由 V2Ray 官方提供。该脚本可以在 Debian 系列或者支持 Systemd 的 Linux 操作系统使用。

除非你是大佬，或者能够自行处理类似 command not found 的问题，
否则请你使用 Debian 8.x 以上或者 Ubuntu 16.04 以上的 Linux 系统。
本指南默认使用 Debian 8.7 系统作为示范。

首先下载脚本：

$ wget https://toutyrater.github.io/install-release.sh

然后执行脚本安装 V2Ray:

$ bash install-release.sh

PORT:40827
UUID:505f001d-4aa8-4519-9c54-6b65749ee3fb
Created symlink from /etc/systemd/system/multi-user.target.wants/v2ray.service to /lib/systemd/system/v2ray.service.
V2Ray v2.33 is installed.
看到类似于这样的提示就算安装成功了。如果安装不成功脚本会有红色的提示语句，
这个时候你应当按照提示除错，除错后再重新执行一遍脚本安装 V2Ray。
对于错误提示如果看不懂，使用翻译软件翻译一下就好。

在安装完 V2Ray 之后，修改配置文件重启 V2Ray 即可，配置文件路径为 /etc/v2ray/config.json。

使用以下命令启动 V2Ray:

$ sudo systemctl start v2ray


# 修改配置文件重启


- 客户端配置文件
-
- {
-  "port": 1080,
-  "log": {
-    "access": ""
-  },
-  "inbound": {
-    "protocol": "socks",
-    "settings": {
-      "auth": "noauth",
-      "udp": false,
-      "ip": "127.0.0.1"
-    }
-  },
-  "outbound": {
-    "protocol": "vmess",
-    "settings": {
-      "vnext": [
-        {
-            "address": "1.2.3.4", //服务器VPS IP地址
-           "port": 12345, //通讯端口，客户端和服务端一致，自定 
-           "users": [
-            {
-              "id": "293c95b0-a4a2-47d5-b6eb-da8e6894e7b8", //UUID 在这个网站随机生成一个，https://www.uuidgenerator.net， 要保证client和server配置一个UUID
-              "alterId": 55 // 这个自定，数字范围0-100，但要保证客户端和服务端一致
-            }
-          ]
-        }
-      ]
-    }
-  },
-  "outboundDetour": [
-    {
-      "protocol": "freedom",
-      "settings": {},
-     "tag": "direct"
-    }
-  ],
-  "routing": {
-    "strategy": "rules",
-    "settings": {
-      "rules": [
-        {
-          "type": "field",
-          "port": "54-79",
-          "outboundTag": "direct"
-        },
-       {
-          "type": "field",
-          "port": "81-442",
-          "outboundTag": "direct"
-        },
-        {
-          "type": "field",
-          "port": "444-65535",
-          "outboundTag": "direct"
-        },
-        {
-          "type": "field",
-          "domain": [
-            "gc.kis.scr.kaspersky-labs.com"
-          ],
-          "outboundTag": "direct"
-        },
-       {
-          "type": "chinasites",
-          "outboundTag": "direct"
-        },
-        {
-          "type": "field",
-          "ip": [
-            "0.0.0.0/8",
-            "10.0.0.0/8",
-            "100.64.0.0/10",
-            "127.0.0.0/8",
-            "169.254.0.0/16",
-            "172.16.0.0/12",
-            "192.0.0.0/24",
-            "192.0.2.0/24",
-            "192.168.0.0/16",
-            "198.18.0.0/15",
-            "198.51.100.0/24",
-            "203.0.113.0/24",
-            "::1/128",
-            "fc00::/7",
-            "fe80::/10"
-          ],
-          "outboundTag": "direct"
-        },
-        {
-          "type": "chinaip",
-          "outboundTag": "direct"
-        }
-     ]
-    }
-  }
- }
- v2ray-server.json
- 服务端配置文件，(基本不需要修改)
-
-
-{
-  "port": 12345, //通讯端口，客户端和服务端一致，自定
-  "log" : {
-    "access": "/var/log/v2ray/access.log", 
-    "error": "/var/log/v2ray/error.log",  
-    "loglevel": "warning"                  
-  },
-  "inbound": {
-    "protocol": "vmess",    
-    "settings": {
-      "clients": [
-        {
-          "id": "293c95b0-a4a2-47d5-b6eb-da8e6894e7b8",  //UUID 在这个网站随机生成一个，https://www.uuidgenerator.net， 要保证client和---server配置一个UUID
-          "alterId": 55,  // 这个自定，数字范围0-100，但要保证客户端和服务端一致
-          "level": 1  // 官方说明，0 共享VPS, 1 自用VPS
-       }
-      ]
-    }
-  },
-  "outbound": {
-    "protocol": "freedom",  
-    "settings": {}
-  },
-  "inboundDetour": [
-    {
-      "protocol": "shadowsocks",   // 此段为支持SS协议部分
-      "port": 1234, 
-      "settings": {
-        "method": "chacha20", // 加密协议支持aes-256-cfb, aes-128-cfb, chacha20 (V2Ray 1.9+), chacha20-ietf (V2Ray 1.9+)
-        "password": "v2ray",     
-        "udp": false             //是否支持UDP中转
-      }
-    }
-  ],
-  "outboundDetour": [
-    {
-      "protocol": "blackhole",  
-      "settings": {},
-      "tag": "blocked"
-    }
-  ],
-  "routing": {
-    "strategy": "rules",
-    "settings": {
-      "rules": [
-        {
-          "type": "field",  
-          "ip": [
-            "0.0.0.0/8",
-            "10.0.0.0/8",
-            "100.64.0.0/10",
-            "127.0.0.0/8",
-            "169.254.0.0/16",
-            "172.16.0.0/12",
-            "192.0.0.0/24",
-            "192.0.2.0/24",
-            "192.168.0.0/16",
-            "198.18.0.0/15",
-            "198.51.100.0/24",
-            "203.0.113.0/24",
-            "::1/128",
-            "fc00::/7",
-            "fe80::/10"
-          ],
-          "outboundTag": "blocked"
-        }
-      ]
-    }
-  }
-}
