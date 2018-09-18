---
title: 微信小程序 request域名配置好之后，还是报错配置的域名不在request合法域名中
categories: 微信小程序
tags: 
  - 微信小程序
  - nodejs
  - nginx
  - 反向代理
---

自己尝试着用nodejs撘个后台服务的时候，本地和postman测试都ok，但是在小程序中使用的时候，报错说<font color="#f44336">"配置的域名不在request合法域名中"</font>
<img src="/images/minapp1.png">

明明已经配置好了的啊，看着报错信息，仔细对比了一下两个url请求地址，发现是由于<font color="#f44336">链接中添加了端口号</font>造成的报错，<font color="#f44336">去掉端口号后不报“合法域名”的错误了</font>，但是也请求不到需要的数据了。
<!--more--> 

想到后台服务中是有端口号的，必须用<font color="#f44336">url+端口号</font>进行请求才可以获取到数据，遂直接更改后台项目中的端口号为443，但是问题依然没有解决，最后突然想到用<font color="#f44336">nginx反向代理</font>试试，把后台服务端口号改回之前的，然后nginx反向代理到此端口号，保存修改
```
location / {
    proxy_pass https://127.0.0.1:8081;
}
```

**重启nginx服务**
```
service nginx restart
```

**请求成功**
<img src="/images/minapp3.png">

***ps：小程序中的网络请求不能有端口号且必须是https请求***