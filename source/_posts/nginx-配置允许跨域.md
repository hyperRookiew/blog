---
title: Nginx日常使用中的配置积累
categories: [技术]
tags:
  - Nginx
date: 2021-11-25 15:23:00
---

### 配置https
```
server {
  listen        443;
  server_name   zwboy.cn;

  ssl           on;
  ssl_certificate_key   ./cert/zwboy.cn.prikey.key
  ssl_certificate       ./cert/zwboy.cn.pem;

  location / {
    proxy_pass http://localhost:8080;
    proxy_set_header HOST $host;
  }
}



# http跳转到https
server {
  listen 80 default_server;
  listen [::]:80 default_server; # ip访问
  server_name zwboy.cn;

  return 302 https://$server_name$request_uri; # 302跳转到https
}
```
<!--more-->


### 配置支持http2
```
server {
  listen       443 http2; # 加上一个http2就可以了
  server_name  zwboy.cn;

  http2_push_preload  on; # 开启支持http2的推送特性

  ssl           on;
  ssl_certificate_key   ./cert/zwboy.cn.prikey.key
  ssl_certificate       ./cert/zwboy.cn.pem;

  location / {
    proxy_pass http://127.0.0.1:8888;
    proxy_set_header Host $host;
  }
}
```

### 配置nginx缓存
```
proxy_cache_path cache levels=1:2 keys_zone=my_cache:10m;

server {
  listen       80;
  # listen       [::]:80 default_server;
  server_name  zwboy.cn;

  # return 302 https://$server_name$request_uri;

  location / {
    proxy_cache my_cache;
    proxy_pass http://127.0.0.1:8888;
    proxy_set_header Host $host;
  }
}

```
### 配置跨域
```
server{
    location / {  
      add_header Access-Control-Allow-Origin *;
      add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
      add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';
      if ($request_method = 'OPTIONS') {
          return 204;
      }
  }
}
```

### 配置多个host的代理
```
server {
        listen 80;  # 设置Nginx对外监听80端口
        server_name *.domain.cn;  # 绑定到该服务器的域名
        if ( $http_host ~* "^(.*?)\.domain\.cn" ) {  # 对http_host进行正则匹配，解析domain
                set $domain $1;
        }
        location / {
                proxy_set_header        X-Real-IP       $remote_addr;
                proxy_set_header        Host            $http_host;
                # 分别处理各个domain
                if ( $domain ~* "www" ) {
                        proxy_pass http://localhost:82;  # 通过proxy_pass 进行代理转发
                }
                if ( $domain ~* "gitlab" ) {
                        proxy_pass http://localhost:82;
                }
                if ( $domain ~* "jenkins" ) {
                        proxy_pass http://localhost:9090;
                }
        }
}
```

### 部署React前端单页应用
```
server {
    listen 8083;
    root /home/name/..../build; # 前端build完成之后的静态资源路径
    index index.html index.htm;

    location / {
        try_file $uri $uri/ /index.html;  # url 切换时始终返回index.html
    }

    # 其他配置
    # 图片样式缓存1年
    location ~* /app.*\.(js|css|png|jpg)$ {
       access_log off;
        expires    365d;
    }
    # html/xml/json 文件不缓存
    location ~* /app.*\.(?:manifest|appcache|html?|xml|json)$ {
        expires    -1;
    }
}
```

###  server_name
```
server{
  server_name primary.zwboy.cn blog.zwboy.cn;
  server_name_in_redirect off;
  return 302 /redirect;
}
```

### nginx http模块的11个阶段
> 简化的阶段：1.读取request头，2.检查配置块，3.流量控制（是否超出连接限制，速率控制），4.鉴权（是否盗链），5.生成内容（5.1作为代理服务器，去上有请求数据，5.2，内部生成数据），6.Response 过滤（压缩，SSL等），7.日志处理

- 实际是按照以下11个阶段顺序进行的

##### POST_READ
- 读取request headers之后
- realip，拿到用户的真实ip，可以用于限流等
##### SERVER_REWRITE
- rewite模块
##### FIND_CONFIG
- nginx模块本身
##### REWRITE
##### POST_REWRITE
- rewrite之后的阶段
##### PREACCESS
- 确认访问权限前的阶段
- limt_conn，并发连接限制
- limt_req，每秒请求数限制
##### ACCESS
- 判断能不能访问
- auth_basic，用户名，密码
- access，根据ip
- auth_request，根据第三方服义判断
##### POST_ACCESS
- 访问之后阶段
##### PRECONTENT
- 连接前阶段
- try_files
##### CONTENT
- index
- autoindex
- concat
##### LOG
- access_log

### realip
```bash
server{
  server_name domain.cn;

  set_real_ip_from 111.111.112.1;
  real_ip_recursive on;
  real_ip_header X-Forwardwd-For;  # realip可以来自Http的头，X-Forwardwd-For和Real-IP

  location / {
    return 200 "Client real ip: $remote_addr\n";
  }
}
```

### rewrite
