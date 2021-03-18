> 洞态IAST的版本、功能、架构和部署方案

洞态IAST有两个版本：
- SaaS版本
- 开源版本（私有化部署版本）


### 洞态IAST简介
洞态IAST包含前端项目和后端API，前端项目基于`Vue`、`TypeScript`开发，后端项目基于`Django`、`Django Rest Framework`开发。包括：策略搜索、项目列表、项目详情、新建项目、导出项目漏洞、应用漏洞管理、第三方组件管理、Agent管理、Hook规则管理、用户管理等功能。

SaaS版本地址：[http://aws.iast.huoxian.cn:8000/](http://aws.iast.huoxian.cn:8000/)


### 架构图
![lingzhi-架构图](../../doc/assets/deploy/framework.png)


### 开源版本涉及项目

开源版本与SaaS版本功能完全一致，开源版本适合对现有检测能力、产品功能等不满意，想要贡献代码或二次开发的甲方、IAST技术爱好者。开源版本需要自行申请，申请条件见[下文](/doc/deploy/versions?id=申请条件)

**基础服务**

- lingzhi-agent-java，java版本的agent代码实现，用于从Java应用系统中采集数据
- lingzhi-web，IAST产品的前端项目
- lingzhi-webapi，IAST产品的api接口，主要用于与`lingzhi-web`项目交互
- lingzhi-agent-server，IAST产品的agent端api接口，用于与不同语言agent的数据交互
- lingzhi-engine，IAST产品的漏洞检测引擎，污点调用图梳理、污点链搜索、漏洞检测等功能均在该项目中实现

基础服务存在依赖关系，请依次部署`lingzhi-agent-server`、`lingzhi-engine`、`lingzhi-webapi`、`lingzhi-web`

#### 基于Docker部署洞态IAST

**依赖：**`Redis`、`mysql`、`Docker`或`K8s`

**1. Redis部署**
- 根据redis官方文档下载并安装redis：[https://redis.io/documentation](https://redis.io/documentation)

如果已经有redis服务，直接复用即可；如有需要，可直接使用火线官方的`redis`镜像进行部署

**2. MySql部署**
- 下载并安装mysql5.7版本，下载地址：https://dev.mysql.com/downloads/installer/
- 设置数据库帐号密码：root password
- 下载并安装客户端连接工具Navicat,下载地址：http://www.formysql.com/xiazai.html
- 创建数据库`iast-webapi`，选择数据库引擎`innodb`
- 导入webapi的数据库脚本，脚本联系[技术支持](/doc/aboutus/support)索要

**3. lingzhi-engine服务部署**

创建本地配置文件，例如：/tmp/config.ini
<details>
    <summary>点击查看<code>config.ini</code>文件内容</summary>
<pre><codes>

[mysql]
host = localhost
port = 3306
name = iast_webapi
user = root
password = password

[redis]
host = host
port = 6379
password = password
db = 0

</codes></pre>
</details>

拉取docker镜像：`docker pull huoxian/lingzhi-engine`

启动docker镜像：`docker run -d --name lingzhi-engine -p 8081:8000 --restart=always -v /tmp/config.ini:/opt/iast/engine/conf/config.ini huoxian/lingzhi-engine`

**4. lingzhi-agent-server服务部署**

创建本地配置文件，例如：/tmp/config.ini
<details>
    <summary>点击查看<code>config.ini</code>文件内容</summary>
<pre><codes>

[mysql]
host = localhost
port = 3306
name = iast_webapi
user = root
password = password

[redis]
host = host
port = 6379
password = password
db = 0

[engine]
url = http://127.0.0.1:8081

</codes></pre>
</details>

拉取docker镜像：`docker pull huoxian/lingzhi-apiserver`

启动docker容器：`docker run -d --name lingzhi-apiserver -p 8082:8000 --restart=always -v /tmp/config.ini:/opt/iast/apiserver/conf/config.ini huoxian/lingzhi-apiserver`

**5. lingzhi-webapi服务部署**

创建本地配置文件，例如：/tmp/config.ini
<details>
    <summary>点击查看<code>config.ini</code>文件内容</summary>
<pre><codes>

[mysql]
host = localhost
port = 3306
name = iast_webapi
user = root
password = password

[redis]
host = host
port = 6379
password = password
db = 0

[engine]
url = http://127.0.0.1:8081

[apiserver]
url = http://127.0.0.1:8082

</codes></pre>
</details>

拉取docker镜像：`docker pull huoxian/lingzhi-webapi`

创建并启动docker容器：`docker run -d --name lingzhi-webapi -p 8082:8000 --restart=always -v /tmp/config.ini:/opt/iast/webapi/conf/config.ini huoxian/lingzhi-webapi`


**6. lingzhi-web服务部署**

使用`nginx.conf`文件的内容创建nginx配置文件`/opt/lingzhi/nginx.conf`，修改其中的**http://lingzhi-api-svc:80**为**lingzhi-webapi**服务的地址

<details>
    <summary>点击查看<code>nginx.conf</code>文件内容</summary>
<pre><codes>

#user  nobody;
 worker_processes  1;
 events {
     worker_connections  1024;
 }
 http {
     include       mime.types;
     default_type  application/octet-stream;
     sendfile        on;
     keepalive_timeout  65;

     #gzip  on;
     gzip on;
     gzip_min_length  5k;
     gzip_buffers     4 16k;
     #gzip_http_version 1.0;
     gzip_comp_level 3;
     gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
     gzip_vary on;

     server {
         listen  80;
         server_name iast.huoxian.cn aws.iast.secnium.xyz  *.cn-north-1.elb.amazonaws.com.cn aws.iast.huoxian.cn;
         client_max_body_size 100M;
         location / {
             root /usr/share/nginx/html;   #站点目录
             index index.html index.htm;   #添加属性。
             try_files $uri $uri/ /index.html;
         }

           location /api/ {
             proxy_read_timeout 60;
             proxy_pass http://lingzhi-api-svc:80/api/;
           }

           location /upload/ {
             proxy_pass http://lingzhi-api-svc:80/upload/;
           }

         location = /50x.html {
             root   /usr/share/nginx/html;
         }
     }
 }

</codes></pre>
</details>

拉取docker镜像：`docker pull huoxian/lingzhi-web`

创建并启动docker容器：`docker run -d --name lingzhi-web -p 80:80 --restart=always -v /opt/lingzhi/nginx.conf:/etc/nginx/nginx.conf huoxian/lingzhi-web`


#### 申请条件

申请人员类型：
- **洞态IAST** 无法满足需求，需要定制开发
- 想要贡献代码

#### 申请方式
- 联系[技术支持](/doc/aboutus/support)
