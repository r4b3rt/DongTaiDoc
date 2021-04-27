#### 手动修改
> 非集群模式

##### 1.安装Agent.jar

登陆 [IAST平台](http://iast.huoxian.cn:8000/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 2.部署Agent

进入`WebLogic`目录，打开`bin/startWebLogic.sh`文件，找到`JAVA_OPTIONS="${SAVE_JAVA_OPTIONS}"`所在行，在该行的下面增加一行
```shell
JAVA_OPTS="$JAVA_OPTS "-javaagent:/opt/jboss/iast/agent.jar" "-Dproject.name=<project name>
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。


> 集群模式

#### 方式一、通过WebLogic的console控制台

访问weblogic的console，例如：

1.找到“环境”下的“服务器”，然后在服务器列表中点击需要安装agent的服务器，如：AdminServer

![adminserver.png](../assets/deploy/weblogic/adminserver.png)

2.进入服务器详情，点击“服务器启动”，在下方的参数一栏中填入javaagent的参数
```shell
JAVA_OPTS="$JAVA_OPTS "-javaagent:/opt/jboss/iast/agent.jar" "-Dproject.name=<project name>
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。


![adminserver.png](../assets/deploy/weblogic/boot.png)

3.重启服务器，使配置生效

![adminserver.png](../assets/deploy/weblogic/restart.png)

#### 方式二、通过修改weblogic的config.xml文件

##### 1.安装Agent.jar

登陆 [IAST平台](http://iast.huoxian.cn:8000/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 部署Agent
找到`/u01/oracle/weblogic/user_projects/domains/base_domain/config`目录下的`config.xml`文件，定位到`<server-start>`标签下的`<arguments>`标签，在标签内添加如下配置：
`-javaagent:/path/to/agent.jar -Dproject.name=<project name>`

注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。
