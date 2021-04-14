> JBossAS 6

#### Linux-手动修改

##### 1.安装 agent.jar

登陆 [IAST平台](http://iast.huoxian.cn:8000/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 2.部署
进入JBoss容器的主目录，在`bin/run.sh`文件中找到`# Setup JBoss specific properties`所在行，在该行的下面插入如下行：

```shell
JAVA_OPTS="$JAVA_OPTS "-javaagent:/opt/jboss/iast/agent.jar" "-Dproject.name=<project name>
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

> JBossAS 7、JBossWildfly

#### Linux-手动修改

##### 1.安装 agent.jar

登陆 [IAST平台](http://iast.huoxian.cn:8000/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 2.部署

进入JBoss容器的主目录，根据当前服务器的启动类型：standalone、domain修改对应的配置文件

**Standalone模式**
打开`bin/standalone.sh`文件，定位`# Display our environment`所在的行，在其上方插入自定义配置，如下：

```shell
JAVA_OPTS="$JAVA_OPTS "-javaagent:/opt/jboss/iast/agent.jar" "-Dproject.name=<project name>
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。


**domain模式**

当前版本为社区版本，不建议在domain模式下安装Agent。
