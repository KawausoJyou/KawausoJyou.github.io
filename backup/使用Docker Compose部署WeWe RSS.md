讨厌微信，但微信公众号有部分优质内容，为了可以使用RSS订阅微信公众号，一直在寻找可行的方法。
其中，RSSHub虽然提供多个订阅渠道，但年久失修都已经失效。又有多个平台提供付费订阅，经过比较选择了某一平台使用了半年多。
最近发现新项目[WeWe RSS](https://github.com/cooderl/wewe-rss)可自行搭建使用，遂尝试搭建使用以替代付费订阅平台。

### 部署

1. 创建存储映射数据库文件的文件夹；
2. 按需修改WeWe RSS文档提供的[docker-compose.yml](https://github.com/cooderl/wewe-rss/blob/main/docker-compose.sqlite.yml)文件，其中`./data`替换为`创建存储映射数据库文件的文件夹`路径；

```
version: '3.9'

services:
  app:
    image: cooderl/wewe-rss-sqlite:latest
    ports:
      - 4000:4000
    environment:
      # 数据库连接地址
      # - DATABASE_URL=file:../data/wewe-rss.db
      - DATABASE_TYPE=sqlite
      # 服务接口请求授权码
      - AUTH_CODE=123567
      # 提取全文内容模式
      # - FEED_MODE=fulltext
      # 定时更新订阅源Cron表达式
      # - CRON_EXPRESSION=35 5,17 * * *
      # 服务接口请求限制，每分钟请求次数
      # - MAX_REQUEST_PER_MINUTE=60
      # 外网访问时，需设置为服务器的公网 IP 或者域名地址
      # - SERVER_ORIGIN_URL=http://localhost:4000

    volumes:
      # 映射数据库文件存储位置，容器重启后不丢失
      - ./data:/app/data
```

3. 按需修改WeWe RSS文档提供的[nginx.example.conf](https://raw.githubusercontent.com/cooderl/wewe-rss/main/assets/nginx.example.conf)文件，并使用`sudo certbot --nginx`命令申请SSL证书；
3. `docker compose up -d`启动。

### 使用

1. 访问配置的地址，进入账号管理，点击添加账号，微信扫码登录微信读书账号（建议使用小号）;
2. 进入公众号源，点击添加，通过提交微信公众号分享链接，订阅微信公众号。 