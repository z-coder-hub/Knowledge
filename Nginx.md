## 下载Nginx

* 搜索引擎 Google、bing等
  * **Nginx download for ubuntu**



## 挂载项目

```nginx
server {
        listen 8080;
        server_name 39.102.213.173;

        # 设置 React 应用的根目录
        root /football/football-frontend;

        index index.html;
        # 配置静态文件路径
        location / {
                try_files $uri $uri/ /index.html;  # 所有请求都返回 index.html
        }

        # 配置静态资源文件（如 JS, CSS, 图片等）
        #location /static/ {
        #       alias /football/football-frontend/static/;
        #}

        # 配置 API 请求的代理转发
        location /api/ {
                proxy_pass http://39.102.213.173:3000/;  # 后端 API 代理
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }

        # 日志配置
        access_log /var/log/nginx/my-app-access.log;
        error_log /var/log/nginx/my-app-error.log;
}
```

