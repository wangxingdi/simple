# simple
一个简单的hexo主题

# How use

# 先决条件

## nginx

### ubuntu18.04 LTS安装nginx

* 执行命令进行安装
  * sudo -s
  * sudo apt update
  * sudo apt install nginx

* nginx相关操作
  * 启动：sudo systemctl start nginx
  * 停止：sudo systemctl stop nginx
  * 状态：sudo systemctl status nginx
  * 重启：sudo systemctl restart nginx
  * 重载：sudo systemctl reload nginx

### nginx配置

* 为了方便操作，在/home/ubuntu/路径下创建一个文件夹作为网站根目录，比如google.com。
* 在/etc/nginx/conf.d/路径下创建一个配置文件，比如google.com.conf，加入如下配置：

```
server {
    listen  80;   #监听端口
    server_name  www.google.com;  #绑定域名
    index  index.html;  #默认显示页面
    root /home/ubuntu/google.com;  #网站根目录
}

server {
    server_name google.com;
    return 301 $scheme://www.$host$request_uri;  # 301跳转www
}
```



* 修改hexo的_config.yml，修改
  * theme：simple
* 修改hexo的package.json，加入
  * "hexo-generator-feed": "^3.0.0"
  * "hexo-generator-sitemap": "^2.0.0"
  * "hexo-renderer-pug": "^1.0.0"
  * "hexo-wordcount-sy": "^6.0.4"
* 执行命令
  * npm install
  * hexo clean
  * hexo g
  * hexo s

