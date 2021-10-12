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

## vps和git同步
我期望的结果是：在任何一台电脑上使用markdown写博客，只需要将md文件上传到github即可，后续的发布使用linux的定时任务完成

### 更新theme脚本hexo-theme-pull.sh

```sh
#!/bin/bash

cd /home/ubuntu/hexo-blog/hexo-blog/themes/simple
result=`sudo git pull`

if [ "$result" == "Already up to date." ]; then
        echo "===git no update required"
else
        echo "===git pull successful"
fi
```

### 更新并发布post脚本hexo-post-public.sh
```sh
#!/bin/bash

cd /home/ubuntu/hexo-blog/hexo-blog/source/_posts/blog
result=`sudo git pull`

if [ "$result" == "Already up to date." ]; then
        echo "===no post publish"
else
        cd /home/ubuntu/hexo-blog/hexo-blog
        sudo hexo clean && sudo hexo g
        if [ $? -eq 0 ]; then
                sudo cp -r /home/ubuntu/hexo-blog/hexo-blog/public/* /home/ubuntu/wangxingdi.com/
                echo "===post publish successful"
        else
                echo "===hexo g is fail"
        fi
fi
```

### 定时任务
* cd /var/spool/cron/crontabs
* sudo vim ubuntu
* 添加一行：0 0 2 * * ? bash /home/ubuntu/sh/hexo-post-publish.sh >> /home/ubuntu/log/cron.log
* sudo service cron reload
* mkdir home/ubuntu/log

#### 常用命令
* service cron start  /*启动服务*/
* service cron stop /*关闭服务*/
* service cron restart /*重启服务*/
* service cron reload /*重新载入配置*/
* service cron status /*查看crond状态*/ 
* crontab -l /*查看当前用户任务列表 */

