### 手动安装-Linux

##### 1.安装Agent.jar

登陆 [IAST平台](http://iast.huoxian.cn:8000/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 2.部署Agent

1.进入`tomcat`所在目录

2.在 `tomcat/bin` 目录下编辑 `catalina.sh` 文件，加入参数：
```shell
CATALINA_OPTS=-javaagent:/path/to/server/agent.jar" "-Dproject.name=<project name>
```

![tomact_config_catalina.png](../../../doc/assets/deploy/manual/tomcat_config_catalina.png)

- 注意：`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

