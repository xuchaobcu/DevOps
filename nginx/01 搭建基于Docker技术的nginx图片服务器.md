



# 搭建基于Docker技术的Nginx图片服务器

***需求来源：***

基于Selenium的UI测试结果反馈不及时，希望能提供浏览器随时查看测试结果的在线服务。



***参考文档：***

http://www.runoob.com/docker/docker-install-nginx.html



***自我实践：***

**实践步骤：**

1. 安装nginx镜像

2. 运行nginx容器

3. 验证nginx文件服务器功能


**上机操作：**

1. **查找Nginx镜像**

​      docker pull nginx

<img src="./picture/01/01 安装nginx镜像.png"/>



2. **运行Nginx容器**

-  创建nginx需要的目录

​       mkdir -p /mynginx/logs /mynginx/conf.d 

​      说明：logs目录映射为nginx容器的日志目录，conf目录中的配置文件映射为nignx容器的配置文件目上录 

- 创建图片服务器存储目录

   mkdir -p /mynginx/www

   <img src="./picture/01/02 创建nginx目录.png"/>

- 创建nginx.conf文件

  vim /mynginx/conf/nginx.conf

  ```sh
  
  #user  nobody;
  worker_processes  1;
  
  #error_log  logs/error.log;
  #error_log  logs/error.log  notice;
  #error_log  logs/error.log  info;
  
  #pid        logs/nginx.pid;
  
  
  events {
      worker_connections  1024;
  }
  
  
  http {
      include       mime.types;
      default_type  application/octet-stream;
  
      #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
      #                  '$status $body_bytes_sent "$http_referer" '
      #                  '"$http_user_agent" "$http_x_forwarded_for"';
  
      #access_log  logs/access.log  main;
  
      sendfile        on;
      #tcp_nopush     on;
  
      #keepalive_timeout  0;
      keepalive_timeout  65;
  
      #gzip  on;
  
      server {
          listen       80;
          server_name  localhost;
  
          #charset koi8-r;
  
          #access_log  logs/host.access.log  main;
  
          location / {
              root   html;
              index  index.html index.htm;
          }
  
          location /www {
              root /mynginx;
              autoindex on;
              autoindex_exact_size on;
              autoindex_localtime on;
              if ($request_filename ~* ^.*?\.(png|doc|pdf)$){
                  add_header Content-Disposition: attachment;
              }
          }
  
  
          #error_page  404              /404.html;
  
          # redirect server error pages to the static page /50x.html
          #
          error_page   500 502 503 504  /50x.html;
          location = /50x.html {
              root   html;
          }
  
          # proxy the PHP scripts to Apache listening on 127.0.0.1:80
          #
          #location ~ \.php$ {
          #    proxy_pass   http://127.0.0.1;
          #}
  
          # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
          #
          #location ~ \.php$ {
          #    root           html;
          #    fastcgi_pass   127.0.0.1:9000;
          #    fastcgi_index  index.php;
          #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
          #    include        fastcgi_params;
          #}
  
          # deny access to .htaccess files, if Apache's document root
          # concurs with nginx's one
          #
          #location ~ /\.ht {
          #    deny  all;
          #}
      }
  
  
      # another virtual host using mix of IP-, name-, and port-based configuration
      #
      #server {
      #    listen       8000;
      #    listen       somename:8080;
      #    server_name  somename  alias  another.alias;
  
      #    location / {
      #        root   html;
      #        index  index.html index.htm;
      #    }
      #}
  
  
      # HTTPS server
      #
      #server {
      #    listen       443 ssl;
      #    server_name  localhost;
  
      #    ssl_certificate      cert.pem;
      #    ssl_certificate_key  cert.key;
  
      #    ssl_session_cache    shared:SSL:1m;
      #    ssl_session_timeout  5m;
  
      #    ssl_ciphers  HIGH:!aNULL:!MD5;
      #    ssl_prefer_server_ciphers  on;
  
      #    location / {
      #        root   html;
      #        index  index.html index.htm;
      #    }
      #}
  
  }
  
  ```

- 上传测试文件

  <img src="./picture/01/03 上传测试文件.png"/>

- 启动nginx容器

  ```sh
  docker run -p 80:80 --name mynginx -v $PWD/www:/www -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf -v $PWD/logs:/wwwlogs  -d nginx
  ```

  

  <img src="./picture/01/03 启动nginx容器.png"/>



<img src="./picture/01/03 启动nginx容器2.png"/>


3. **验证Nginx文件服务器功能**

    从客户端的浏览器，访问docker中nginx提供的服务：

    http://192.168.192.132/www/

    <img src="./picture/01/05 客户端浏览器访问nginx服务.png"/>

    点击其中的文件，可以在线预览：

    <img src="./picture/01/06 测试nginx文件服务器功能1.png"/>

    

    <img src="./picture/01/06 测试nginx文件服务器功能2.png"/>

大功告成。