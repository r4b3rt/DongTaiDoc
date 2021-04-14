#### 手动安装

##### 1.安装Agent.jar

登陆 [IAST平台](http://iast.huoxian.cn:8000/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

#### 2.配置WebSphere服务器
进入WebSphere WEB端的管理后台，在控制台左侧的导航栏里，选择`Servers -> Server Types -> WebSphere Application Server`，进入应用列表界面：

![app.png](../../assets/deploy/websphere/app.png)

选择需要安装agent的应用（以server1为例），点击进入管理页面。在新页面向下翻，找到`Server Infrastructure -> Process definition`，并点击进入：

![server1.png](../../assets/deploy/websphere/server1.png)

点击`Additional Properties -> Java Virtual Machine`进入JVM启动参数编辑界面

![jvmarg.png](../../assets/deploy/websphere/jvmarg.png)

找到`Generic JVM arguments`选项，开始编辑并在里面填写以下内容并保存`-javaagent:/path/to/agent.jar -Dproject.name=<project name>`

注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

重启对应修改后的server应用

