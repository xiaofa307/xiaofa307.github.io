---
layout: post
title: nginx配置dmz代理
categories: ssh
tags: ssh
---

### 前言

公司的服务器部署在内网,通过dmz服务器的代理来访问外网,但是遇到已经开通了外网权限还是访问不通的问题,通过wireshark分析解决。

1. 分析dmz服务器需要开通的端口,以访问外网。
2. 代码中原先直接访问外网的接口切换到dmz地址,在dmz上配置nginx,把请求统一代理到外网.


#### 解决过程

	以铁友网data.99pto.com为例,项目里调用他的火车票查询接口。
1. 在dmz上执行"nslookup data.99pto.com" 得到dmz配置的dns,及解析出来的ip。
2. dmz机器开通该ip权限。
3. 代码里调用data.99pto.com的地址变成访问dmz的地址。
3. dmz上配置nginx代理到外网,报连接超时,查看nginx日志发现铁友网禁止通过ip的方式调用接口,只开通了域名调用的方式。
'
	location /train/ {
			proxy_set_header Host $host:$server_port;
      		proxy_pass http://data.99pto.com;
      }
'
4. wireshark对比了本地直接访问和nginx代理的http resquest,发现header一个是data.99pto.com,一个变成了localhost
5. 去除nginx的proxy_set_header.

成功。


