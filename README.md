Nginx base badbot blocker
=====================
屏蔽User-Agent配置，屏蔽垃圾Referrer,垃圾搜索引擎,DDOS和其它IP地址

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

centos example
##yum install GeoIP-devel
##./configure --with-http_geoip_module --with-ipv6 --with-http_map_module=shared
##make && make install

```

## 使用方法

1. git clone 项目所有文件 到你的 /etc/nginx 目录下

```
git clone https://github.com/zhu2688/nginx-base-badbot-blocker /etc/nginx

```
2. nginx.conf (http块)增加配置文件

```
include /etc/nginx/conf.d/*.conf;
```

3. host.conf (server块)增加配置文件

```
include /etc/nginx/bots.d/blockbots.conf;
include /etc/nginx/bots.d/ddos.conf;
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
