# 09网盘安装教程

 

[**config.json](https://github.com/heiyuan0801/09wp/releases/download/1.0/config.json)和[dfan-netdisk-backend-server-linux-amd64](https://github.com/heiyuan0801/09wp/releases/download/1.0/dfan-netdisk-backend-server-linux-amd64)**

这是俩个后端文件上传到宝塔里面

宝塔安装go版本1.24.3

![27b1630c-a533-43b7-b187-53f04621f416.png](attachment:066e47d8-d6e1-4b6d-b2bf-2f62d897fa65:27b1630c-a533-43b7-b187-53f04621f416.png)

![5bbb2aef-0250-483f-a756-75152bbc0be4.png](attachment:27bee32c-d46c-4340-8fb5-cc51f7a6ec81:5bbb2aef-0250-483f-a756-75152bbc0be4.png)

前端文件[dist.zip](https://github.com/heiyuan0801/09wp/releases/download/1.0/dist.zip)

宝塔创建文件夹 解压到里面

创建一个html项目

![dd679234-deb8-4321-8c1d-74ca1ec0364b.png](attachment:75005118-98fd-401a-a3d2-df2fbe4f1a92:dd679234-deb8-4321-8c1d-74ca1ec0364b.png)

设置宝塔伪静态

```jsx
# 保留静态资源 & 接口直通
location ~* \.(js|css|png|jpg|jpeg|gif|svg|ico|webp|json|txt|woff|woff2|ttf|eot)$ {
    try_files $uri =404;
    expires 30d;
    access_log off;
}

# API、Sitemap、RSS 等后端接口不要走前端路由
location ^~ /api/ {
    try_files $uri @backend;
}

location = /sitemap.xml {
    try_files $uri @backend;
}

location = /rss.xml {
    try_files $uri @backend;
}

# 单页应用前端路由：所有其他路径回退到 index.html
location / {
    try_files $uri $uri/ /index.html;
}

# 可选：把后端转发到本机 8080（如果你在同一个站点里反代后端）
location @backend {
    proxy_pass http://127.0.0.1:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

8080端口默认 可以自己修改 需要去config文件修改

数据库的话

```jsx
{
    "http_port": "8080",
    "mysql_dsn": "pan:pan@tcp(127.0.0.1:3306)/pan?charset=utf8mb4&parseTime=True&loc=Local",
    "jwt_secret": "dfan-netdisk-dev-secret",
    "pancheck_base_url": "http://127.0.0.1:6080",
    "redis": {
      "enabled": true,
      "host": "127.0.0.1",
      "port": 6379,
      "username": "",
      "password": "",
      "search_cache_ttl": 60,
      "ping_timeout": 3,
      "connect_timeout_ms": 2000
    }
  }
  
  
```

pan:pan@tcp(127.0.0.1:3306)/pan

第一个数据库名称

第二个密码

第三个数据库名称

填写完保存

然后安装redis 默认开启
