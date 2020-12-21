## 本地mock系统

### 开发基础配置

```
node > 10
```

### 本地host配置

```
127.0.0.1 testmock.xxx.com
```

### nginx配置

```
server {
        listen 80;
        server_name testmock.xxx.com;
        location / {
            proxy_pass http://testmock.xxx.com:8888;
            #   指定允许跨域的方法，*代表所有
            add_header Access-Control-Allow-Methods *;

            #   预检命令的缓存，如果不缓存每次会发送两次请求
            add_header Access-Control-Max-Age 3600;
            #   带cookie请求需要加上这个字段，并设置为true
            add_header Access-Control-Allow-Credentials true;

            #   表示允许这个域跨域调用（客户端发送请求的域名和端口） 
            #   $http_origin动态获取请求客户端请求的域   不用*的原因是带cookie的请求不支持*号
            add_header Access-Control-Allow-Origin $http_origin;

            #   表示请求头的字段 动态获取
            add_header Access-Control-Allow-Headers 
            $http_access_control_request_headers;

            #   OPTIONS预检命令，预检命令通过时才发送请求
            #   检查请求的类型是不是预检命令
            if ($request_method = OPTIONS){
                return 200;
            }
        }
    }
```

### 写在最后

未完待续