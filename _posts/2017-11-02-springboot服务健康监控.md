---
layout: post
title: springboot服务健康监控
categories: springboot
tags: springboot
---

###  怪现象

	近日上线后发现一个奇怪现象,服务器的两个微服务之间调用不稳定,而且呈现一定规律性,比如a服务调用b服务,一次成功,一次失败,又成功,然后再一次失败...  
	失败的时候就报404,找不到b服务,搞不清是什么原因。  
    因为是服务器的问题,本地也没法重新,晚上回家后给服务器配置了远程调试,
	在tomcat的catalina.bat中添加SET CATALINA_OPTS=-Xdebug -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n,然后本地的eclipse配置  
	remote java application,project填本地和远程对应的项目,Host填远程地址,端口号即上面的8000,配置好后开始debug跟踪源代码.  
	
	跟踪后发现springboot的服务间调用是通过feign框架来进行的,feign封装了http请求,请求封装在feign包中的Client类里面的execute方法,convertResponse  
	中的connection.getResponseCode()会执行发起的get请求并返回status,自己又试着在服务器重现了几次,a调用b的时候一次200,一次404这样一直重复,看不出  
	所在,决定把connection这个对象里面的属性都拿出来放到Beyond Compare里面对比一下,随后分别拿了成功和失败的对象进行对比的时候,发现当返回404的时候  
	a调用b的端口不是配置的端口,比如配置文件里面服务b的端口是8080,返回200的时候connection地址也是8080,但是返回404的时候,这个地址就变成了8090,怪不得  
	报错404,根本端口号就不对嘛,然后打开了服务发现的地址进行确认,确实eureka里面的对应b的实例有两个,分别是8080端口的和8090端口的.  
	但是8090这个端口是哪来的呢?配置文件里面的端口是8080啊。
	等到上班和同事确认后才明白了,这个b的端口曾经使用过8090,后来最近的一次部署变成8080了,  
	真相大白了,原来eureka不会主动的清除失效的服务,所以曾经使用过8090端口的b服务还一直注册在里面,并且8090,8080同时存在的时候,这两个端口变成了负载均衡,  
	调用的时候会轮询访问,所以才一次200,一次404,eureka的服务需要配置健康监控才能够定期清除失效的服务。
	
	配置服务的健康监控
	
		eureka.client.healthcheck.enabled = true                                
		eureka.instance.lease-renewal-interval-in-seconds =10
		eureka.instance.lease-expiration-duration-in-seconds =30
	maven依赖添加
	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>

	短短的一段小配置就可以解决问题,却需要大量的时间来定位问题。
---

