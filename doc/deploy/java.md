## 1.下载探针

登陆 [IAST平台](https://iast.huoxian.cn/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`
 
## 2.配置探针

### 2.1 SpringBoot

- 如果使用war包的方式部署，agent的安装方式为具体中间件的安装方式
  
- 如果使用`java -jar app.jar`的方式部署，则在启动命令中增加启动参数`-javaagent:/path/to/agent.jar`即可，如：
```shell
java -javaagent:/path/to/agent.jar -Dproject.name=<project name> -jar app.jar
```

- 注意：`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

### 2.2 Tomcat

- 进入`tomcat`所在目录

- 在 `tomcat/bin` 目录下编辑 `catalina.sh` 文件，加入参数：
  ```shell
  CATALINA_OPTS=-javaagent:/path/to/server/agent.jar" "-Dproject.name=<project name>
  ```

![tomact_config_catalina.png](../assets/deploy/manual/tomcat_config_catalina.png)

- 注意：`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。


### 2.3 JBoss/Wildfly

#### 2.3.1 JBossAS 6

进入JBoss容器的主目录，在`bin/run.sh`文件中找到`# Setup JBoss specific properties`所在行，在该行的下面插入如下行：

```shell
JAVA_OPTS="$JAVA_OPTS -javaagent:/opt/jboss/iast/agent.jar -Dproject.name=<project name>"
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

#### 2.3.2 JBossAS 7、JBossWildfly

进入JBoss容器的主目录，根据当前服务器的启动类型：standalone、domain修改对应的配置文件

**Standalone模式**
打开`bin/standalone.sh`文件，定位`# Display our environment`所在的行，在其上方插入自定义配置，如下：

```shell
JAVA_OPTS="$JAVA_OPTS -javaagent:/opt/jboss/iast/agent.jar -Dproject.name=<project name>"
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

### 2.4 Resin

1.进入Resin的主目录，

2.打开`conf/cluster-default.xml`文件，定位到`<server-default>`所在的行，

3.在该行下面插入
```shell
<jvm-arg>-javaagent:/opt/Resin/iast/agent.jar</jvm-arg>
<jvm-arg>-Dproject.name=<project name></jvm-arg>
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

4.重启Resin

### 2.5 Jetty

1.进入jetty的主目录

2.打开`bin/jetty.sh`文件，找到`Add jetty properties to Java VM options.`所在行

3.在改行的下面插入`JAVA_OPTIONS+=( "-javaagent:/path/to/agent.jar --Dproject.name=<project name>")`

注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

4.重启jetty服务器


### 2.6 WebLogic

#### 2.6.1、通过WebLogic的console控制台

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

#### 2.6.2、通过修改weblogic的config.xml文件

找到`/u01/oracle/weblogic/user_projects/domains/base_domain/config`目录下的`config.xml`文件，定位到`<server-start>`标签下的`<arguments>`标签，在标签内添加如下配置：
`-javaagent:/path/to/agent.jar -Dproject.name=<project name>`

注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。


### 2.7 WebSphere

进入WebSphere WEB端的管理后台，在控制台左侧的导航栏里，选择`Servers -> Server Types -> WebSphere Application Server`，进入应用列表界面：

![app.png](../assets/deploy/websphere/app.png)
 
选择需要安装agent的应用（以server1为例），点击进入管理页面。在新页面向下翻，找到`Server Infrastructure -> Process definition`，并点击进入：

![server1.png](../assets/deploy/websphere/server1.png)

点击`Additional Properties -> Java Virtual Machine`进入JVM启动参数编辑界面

![jvmarg.png](../assets/deploy/websphere/jvmarg.png)

找到`Generic JVM arguments`选项，开始编辑并在里面填写以下内容并保存`-javaagent:/path/to/agent.jar -Dproject.name=<project name>`

注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。

重启对应修改后的server应用

