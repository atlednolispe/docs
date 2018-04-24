Centos 7上使用nginx给scrapy增加密码验证
====================================

[add verify to scrapyd](https://piaosanlang.gitbooks.io/spiders/05day/section5.5.html)
[nginx permission deny](https://www.jianshu.com/p/5aa77c75e903)

## 安装nginx

```
$ yum install epel-release -y
$ yum install nginx -y

$ yum install httpd-tools -y

# 设置密码
# root @ localhost in /etc/nginx/conf.d [8:25:44]
$ htpasswd -c .htpasswd atlednolispe

# 修改为user  root;
$ vim /etc/nginx/nginx.conf.default

# 配置反向代理
$ vim /etc/nginx/nginx.conf
http {
    server {
            listen 6801;
            location / {
                    proxy_pass            http://127.0.0.1:6800/;
                    auth_basic            "Restricted";
                    auth_basic_user_file  /etc/nginx/conf.d/.htpasswd;
            }
    }
}

$ setenforce 0

# 开启防火墙
$ firewall-cmd --zone=public --add-port=6801/tcp --permanent

# 修改scrapy.cfg配置
[deploy]
url = http://ip_address:6801/
project = project_name
username = atlednolispe
password = your_password

$ vim /etc/scrapyd/scrapyd.conf
[scrapyd]
bind_address = 127.0.0.1
http_port   = 6800

# 开启nginx
$ systemctl start nginx
```

## 防火墙的简单配置

```
# 删除规则
$ firewall-cmd --remove-port=6800/tcp --permanent

# 增加规则
$ firewall-cmd --zone=public --add-port=6801/tcp --permanent

# 重启服务
$ systemctl restart firewalld.service

# 列出所有规则
$ firewall-cmd --list-services

# 生效规则
$ firewall-cmd --reload
```