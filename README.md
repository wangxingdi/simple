# simple
一个简单的hexo主题

# How use
下面是全部的操作
## nginx

### ubuntu18.04 LTS安装nginx

* 执行命令进行安装
  * sudo -s
  * sudo apt update
  * sudo apt install nginx

* nginx相关操作指令
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
## hexo

### ubuntu18.04 LTS安装nodejs和npm

* sudo curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
* sudo apt-get install -y nodejs
* reboot

### 检查nodejs和npm的版本号
* nodejs -v
* npm -v

### 安装hexo
* mkdir hexo-blog
* cd hexo-blog
* sudo npm install --unsafe-perm --verbose -g hexo
* sudo hexo init hexo-blog
* 执行命令
  * npm install
  * hexo clean
  * hexo g
  * hexo s
* 修改hexo的_config.yml
  * title: 王兴迪的个人博客
  * subtitle: 脑子是个好东西
  * theme：simple
  * permalink: :title.html
  * skip_render: _posts/blog/README.md
* 修改hexo的package.json，加入
  * "hexo-generator-feed": "^3.0.0"
  * "hexo-generator-sitemap": "^2.0.0"
  * "hexo-renderer-pug": "^1.0.0"
  * "hexo-wordcount-sy": "^6.0.4"
  
## 同步git

### 同步theme
* 进入themes目录
* git init
* git clone https://github.com/wangxingdi/simple.git

### 同步source
* 进入source/_posts
* git init
* git clone https://github.com/wangxingdi/blog.git





