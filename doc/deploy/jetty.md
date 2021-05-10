#### 手工修改

##### 1.安装Agent.jar

登陆 [IAST平台](https://iast.huoxian.cn/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 2.部署
1.进入jetty的主目录

2.打开`bin/jetty.sh`文件，找到`Add jetty properties to Java VM options.`所在行

3.在改行的下面插入`JAVA_OPTIONS+=( "-javaagent:/path/to/agent.jar --Dproject.name=<project name>")`

注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

4.重启jetty服务器

