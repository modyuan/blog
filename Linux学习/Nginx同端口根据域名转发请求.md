# Nginx同端口根据域名转发请求


    #/etc/nginx/site-enable/leanote

```

server {
  listen 80; 
  server_name note.hulituanzi.com;
  location / {
    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Cookie $http_cookie;
    proxy_pass http://localhost:9000/;
  }
  access_log /opt/note_access.log ;
}
```

