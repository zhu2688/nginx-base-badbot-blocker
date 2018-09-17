Nginx base badbot blocker
=====================
屏蔽User-Agent配置，屏蔽垃圾Referrer,恶意扫描程序,垃圾搜索引擎,DDOS和其它恶意IP地址

 - 本项目数据来源 : 
[https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker](https://github.com/mitchellkrogza)
 - 版权所有 Mitchell Krog <mitchellkrog@gmail.com>
 - 后期会加上国内的一些网站并每周更新一次

## 准备
nginx 编译的时候需要加上geo,ipv6,map等模块

```
nginx -VVV | grep "ngx_http_geoip_module"
nginx -VVV | grep "ngx_http_map_module"
nginx -VVV | grep "ngx_http_limit_conn_module"
nginx -VVV | grep "ngx_http_limit_req_module"

### 本人大部分站点运行在 centos6/7 Tenginx(v2.2.2)上 标准nginx版本并没有测试过
### 理论上只要配置不报错，就可以运行
### 本人配置参数
### nginx -VVV
### --with-select_module --with-http_stub_status_module  
    --with-http_ssl_module --with-http_gzip_static_module
    --with-pcre=/usr/local/src/pcre --with-http_geoip_module --with-ipv6
=======

```

## 使用方法

1. git clone 项目所有文件 到你的 /etc/nginx 目录下

```
git clone https://github.com/zhu2688/nginx-base-badbot-blocker /etc/nginx
```

2. nginx.conf (http块)增加配置文件

```
include /etc/nginx/conf.d/*.conf;

### eg:
### http{
###     ...
###     include mime.types;
###     include vhost.conf;
###     ....
###     include servers/site.conf;
###     ....
### }
```

3. host.conf (server块)增加配置文件

```
include /etc/nginx/bots.d/blockbots.conf;
include /etc/nginx/bots.d/ddos.conf;

### eg:
### server {
###     server_name localhost
###     ....
###     include /etc/nginx/bots.d/blockbots.conf;
###     include /etc/nginx/bots.d/ddos.conf;
###     ...
###     ....
### }

```

4. 重启服务

```
/usr/local/nginx/sbin/nginx -t
server nginx restart
```

5. 检查效果

```
### www.domains 替换成自己的域名测试

sh> curl -A "Googlebot" http://www.domains
sh> 有内容输出
sh> curl -A "360Spider" http://www.domains
sh> curl: (52) Empty reply from server

```

## 数据更新
```
cd /etc/nginx 
git pull
```

## 文件结构
```
├── README.md
├── bots.d
│   ├── bad-referrer-words.conf          自定义referrer黑名单配置
│   ├── blacklist-domains.conf           自定义domains黑名单配置
│   ├── blacklist-ips.conf               ips黑名单地址
│   ├── blacklist-user-agents.conf       自定义UA配置
│   ├── blockbots.conf                   主要逻辑处理
│   ├── custom-bad-referrers.conf        自定义referrer白名单配置
│   ├── ddos.conf                        限流配置
│   ├── whitelist-domains.conf           自定义domains白名单配置
│   └── whitelist-ips.conf               自定义ips白名单配置 
└── conf.d
    ├── botblocker-nginx-settings.conf    连接数限制配置相关
    └── globalblacklist.conf              主要配置文件(定义)
```
