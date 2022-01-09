<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

@[TOC]

# <font color=#6495ED size=6 >Nginx Webssh 配置文件</font>

* ### <font color=#000000 size=4 face=粗体>配置文件</font>

  ```conf
  #PROXY-START/
  location  ~* \.(gif|png|jpg|css|js|woff|woff2)$
  {
      proxy_pass http://47.99.185.254:6001;
      proxy_set_header Host $http_host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header REMOTE-HOST $remote_addr;
      proxy_set_header X-Real-PORT $remote_port;
      expires 12h;
  }
  location /
  {
      proxy_pass http://47.99.185.254:6001;
      proxy_http_version 1.1;
      proxy_read_timeout 300;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $http_host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header REMOTE-HOST $remote_addr;
      proxy_set_header X-Real-PORT $remote_port;
      
      add_header X-Cache $upstream_cache_status;
      
      #Set Nginx Cache
      
      
      proxy_ignore_headers Set-Cookie Cache-Control expires;
      proxy_cache cache_one;
      proxy_cache_key $host$uri$is_args$args;
      proxy_cache_valid 200 304 301 302 1m;
  }
  
  #PROXY-END/
  ```

