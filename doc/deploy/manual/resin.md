> Resin 4

#### 手动修改

##### 1.安装Agent.jar

登陆 [IAST平台](http://iast.huoxian.cn:8000/login) 在**部署IAST**中下载洞态IAST的Agent，将agent.jar文件放入WEB服务器（中间件）所在机器上，保证agent.jar文件所在目录具有可写权限，如：`/tmp/`

##### 2.部署agent
1.进入Resin的主目录，

2.打开`conf/cluster-default.xml`文件，定位到`<server-default>`所在的行，

3.在该行下面插入
```shell
<jvm-arg>-javaagent:/opt/Resin/iast/agent.jar</jvm-arg>
<jvm-arg>-Dproject.name=<project name></jvm-arg>
```
注意，`-Dproject.name=<project name>` 为可选参数，`<project name>`与创建的项目名称保持一致，agent将自动关联至项目；如果不配置该参数，需要进入项目管理中进行手工绑定。


4.重启Resin
