> Jar包启动方式

#### 手动安装

##### 1.安装 Agent

登陆 [IAST平台](https://iast.huoxian.cn/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 2.配置 Agent


- 如果使用war包的方式部署，agent的安装方式为具体中间件的安装方式
  
- 如果使用`java -jar app.jar`的方式部署，则在启动命令中增加启动参数`-javaagent:/path/to/agent.jar`即可，如：
```shell
java -javaagent:/path/to/agent.jar -Dproject.name=<project name> -jar app.jar
```

- 注意：`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

