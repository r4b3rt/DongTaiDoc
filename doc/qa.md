
#### 一、如何编译并使用本地Java Agent

> 通过调试发现自己编译 core 和 inject 没用，他的 agent 无论如何还是会从官网上自己下载这两个 jar 包并放到 temp 目录下，不知道是故意的还是写出来的 bug，因为如果从官网上下载，也只是下载 agent，在运行时动态下载 core 和 inject，应该是最开始打算试用，没打算开源？？                                      -- 作者：素十八

**洞态IAST - Java探针分析**

洞态IAST的agent部分由`agent.jar`、`iast-core.jar`和`iast-inject.jar`三部分组成。其中：
- `agent.jar`用于存放配置、定期拉取云端信息、下载并加载核心检测引擎`iast-core.jar`和`iast-inject.jar`
- `iast-inject.jar`用于注入至`BootStrap ClassLoader`，后续在目标应用中调用`iast-core.jar`中的数据采集方法
- `iast-core.jar`是核心jar包，存储数据采集、数据预处理、数据上报等功能

探针启动时，由`agent.jar`检测当前的启动模式，规则如下：
- debug模式为false，从云端下载检测引擎并启动
- debug模式为true，检测本地临时目录中是否存在核心检测引擎
  - 存在，加载本地检测引擎并启动
  - 不存在，从云端下载检测引擎并启动

综上，如果想要试用自己编译的agent，需要指定启动模式为debug`-Ddebug=true`并将`iast-core.jar`和`iast-inject.jar`复制到临时目录中。

**编译及配置方法**

仅需四步即可实现修改`Agent`代码并使用云端环境：
1. 拉取代码：`git clone https://github.com/HXSecurity/DongTai-agent-java.git`
2. 根据需求，修改agent-java中的代码
3. 编译Agent
    3.1 登陆**洞态IAST**云端，前往部署页面获取**Token**和云端服务地址
    3.2 将Token和云端服务地址写入`iast-agent/src/main/resources/iast.properties`文件
    3.3 运行`maven clean package`编译agent
4. 拷贝检测引擎到临时目录
5. 运行应用（以SpringBoot应用为例）：`java -javaagent:/path/to/iast-home/release/iast-agent.jar -Ddebug=true -jar app.jar`

#### 二、关于应用识别/区分

> 检测引擎是如何区分应用的呢？                                                                                    -- 作者：素十八

检测引擎与应用之间的区分可分为：自动识别并区分和手动配置进行区分两类方案，一开始，我们研究了自动识别应用名称，后来发现准确度很难保证，因此最终选择了手动配置的方案。

1. **洞态IAST**支持两种方式配置应用名称：
- 方式一，通过**部署IAST**页面，输入**项目名称**，应用启动时，将自动将探针绑定至对应的项目中
  ![image-20210519091515254](https://huoxian-zone.oss-cn-beijing.aliyuncs.com/imagesimage-20210519091515254.png)
- 方式二，启动应用时，指定JVM参数`-Dproject.name=<project.name>`，如下：
  ```
  java -Dproject.name=<project.name> -jar app.jar
  ```

2. **【讨论】IAST**自动识别项目名称的可选方案
  - ContextPath
  - 当前Request请求所在的物理路径

#### 三、SCA实现原理

> 一个优秀的 IAST 一定有 SCA 一类的功能，简单的实现都是通过收集客户段组件信息，发送到云端通过匹配 CPE，并链接到对应的 CVE/CWE/CNNVD 等                                                                                -- 作者：素十八

**洞态IAST**的SCA功能实现主要分为三部分：
- 收集并上报`JavaWEB`应用中使用的第三方组件
- 与`Maven`官方组件库匹配，获取精确的版本信息
- 与漏洞库对接梳理漏洞（漏洞库数据来源于国内外公开的优质SCA漏洞数据，未通过cpe处理）

1. 收集`JavaWEB`中第三方组件的方案
   根据Java中的类加载机制可知，JVM在加载应用代码类的同时会加载类中引用的第三方组件类。根据这一特性，在`IAST`中获取第三方组件类所在的jar包位置，计为`path1`，然后读取`path1`目录下所有的jar包；最后用`SHA-1`算法计算Jar包的哈希值，并上报至云端
2. 云端与`Maven`官方组件库匹配
  云端根据`SHA-1`哈希值与`Maven`组件库进行匹配，获取对应的`groupId`、`artifactId`和版本信息
3. 云端与漏洞库对接
  拿到组件的版本信息后，与漏洞库对接，判断是否存在cve漏洞

【讨论】关于`SCA`的收集，是否有更好的方式呢？

#### 四、如何获取 token ？

1. 登陆洞态IAST官网，官网地址：https://iast.huoxian.cn/
2. 点击右上角的“部署IAST”，点击“下一步”直到第三步
3. 在“自动下载”中找到参数 Token 
   
#### 五、多个项目共用一个agent的话，后台收集到的漏洞信息会怎么展示？

- 当多个项目共用一个agent时，启动项目时需要添加启动参数“-Dproject.name=project name”，然后在洞态IAST官网新建项目，项目名与参数“-Dproject.name=project name”中的“project name”一致即可自动绑定agent，漏洞信息会展示在各个项目。
  
#### 六、当项目如何绑定多个版本？漏洞数据展示又是怎样的？

1. 在洞态IAST官网新建项目时有填写版本的输入框，填写当前应用版本。当应用更新新版本时，首先进入之前建的项目详情，在“版本”处可以编辑版本，新增版本并且切换到新版本，然后启动新版本应用即可。
2. 不同版本的漏洞数据会展示在其对应版本的项目中。
   
#### 七、项目组件是启动后一次性加载检测完，还是根据应用访问陆续检测？

- 与应用访问有关，而且数据上传回来会有一定的时间间隔
  
#### 八、手动部署时，报错信息提示no module named 'dongtai_models'怎么解决？

- 执行 pip install https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/dongtai-0.1.0.tar.gz
  
#### 九、单机部署时engine和apiserver服务地址怎么配置？

- engine 填 dongtai-engine 服务的内网地址，apiserver 填 dongtai-openapi 服务的内网访问地址
  
#### 十、本地部署时要执行哪些数据库sql文件？

1. 先执行 db.sql
2. 再按日期顺序执行 update.sql
   
#### 十一、为什么启动项目后没有洞态IAST官网检测到探针？

1. 检查自己应用的启动命令，以springboot项目为例，启动命令：java -javaagent:/path/to/agent.jar -Dproject.name=<project name> -jar app.jar，注意先编写-javaagent 再编写 -jar
2. 检查应用所在网络连接是否有问题
   
#### 十二、使用 agent 启动项目后，出现报错信息怎么办？

- agent 在检测时可能会出现报错，通常情况下为正常现象，如果影响到了应用使用，请将报错日志保存并联系技术人员。
  
#### 十三、部署时拉取镜像或者代码失败的原因？

1. 检查网络是否能访问阿里云
2. 检查网络是否能访问 github

#### 十四、本地安装对服务器资源有什么要求吗？

1. 4核8G的服务器
2. 安装docker 

#### 十五、无法访问洞态IAST官方网站？

1. 确认网络是否能正常访问其他网站，例如[火线安全平台](www.huoxian.cn)、[百度](www.baidu.com)。
2. 更换阿里云的 DNS：223.5.5.5 或 223.6.6.6

#### 十六、本地部署洞态IAST时 dongtai-openapi 的 url 从哪里获取？

- docker-compose安装的时候，需要输入两个端口，第二个端口就是openapi的服务端口

#### 十七、系统配置正常请求次数正常没有展示漏洞？

- 系统有延迟
- 取消“已确认”按钮
  
#### 十八、洞态 iast 服务端扫描时支持代理吗？如果iast无法正常访问业务服务接口，但是反向可以，还能正常检测吗？

- 洞态IAST 是被动式检测漏洞，所以不需要主动向业务服务器发起请求
  
#### 十九、洞态IAST的漏洞验证的原理是什么？

- 验证的时候，会根据识别到的攻击参数位置，构造特定的验证poc：../../dongtai 之类的，然后在探针内部进行重放，分析是否能触发相同的污点调用链且触发漏洞。
  
#### 二十、发送验证请求会可能会产生脏数据？

- 不会有脏数据，重放请求有重放标志，在探针端会进行自动拦截

#### 二十一、项目名称，需要唯一吗？

- 项目名称需要是唯一的
  
#### 二十二、自动验证时间长怎么办？

- 自动验证功能需要等探针主动拉取，改一下探针启动参数，-Diast.service.replay.interval=1000，这样就会块很多，但是对系统的CPU占用有影响 
  
#### 二十三、多个app.jar可以共用同一个agent.jar吗？

- 可以，如果需要区分不同的app，通过指定启动参数：project.name即可
  
#### 二十四、（与产品无关）docker中的根目录有一个client 然后在dockerfile中我想用CMD或者ENTRYPOINT让docker启动的时候去后台运行这个client 这个该怎么写？
- 在entrypoint.sh里，用nohup后台运行一下
- CMD "client"
- 标准用法：ENTRYPOINT是容器的入口shell脚本，CMD是shell脚本的参数
  
#### 二十五、师傅，我想提个java的规则，规则定义板块过滤方法规则和危险方法规则在漏洞检测机制上有什么区别呢？

- 过滤方法规则是属于传播节点的一种，用于添加公司内部自定义的危险数据过滤方法，如xss过滤；危险方法是漏洞出发的位置
  
#### 二十六、如果自定义添加规则时，规则怎样和漏洞信息和污点流图关联（xss规则）

- 如果是过滤xss的话，加在过滤方法里，如果是xss的漏洞触发点，加在危险方法里
  
#### 二十七、执行验证的时候，也是根据类型，自动调用集成好的poc验证？

- 是的
  
#### 二十八、iast-core模块，收集机器指纹的模块在哪

- 需要自定义探针的名称，可以修改这个地方：com.secnium.iast.agent.AgentRegister#getAgentToken和com.secnium.iast.core.report.AgentRegisterReport#getAgentToken
  
#### 二十九、规则方法支持正则吗？

- hook方法的位置不支持
- 建议使用IDEA插件添加规则
  
#### 三十、agent重新下载后，探针发生变化了，但是项目名称还是一致的，可以进行漏洞验证吗？

- 需要为同一个agent
  
#### 三十一、关于漏洞验证的一系列问答：
- 问：目前加验证功能的目的是验证漏洞是否存在吗？插庄检测准确率应该比较高，验证的逻辑是判断iast检测是否准确，但是验证的过程是看不到的，即使验证完了，验证的有效性可能也要被验证下，这样验证的功能意义是不是就降低了
- 答：是的，为了验证漏洞是否存在。验证功能可以解决大部分误报，如果自动验证都过不去，基本上也不需要人工验证。但是现在验证功能由探针主动拉取，为了不影响性能，所以每秒钟只处理一次，所以存在验证比较慢的问题，等之后解决性能和验证速度的问题，就好了
- 问：师傅说的逻辑是站在自动验证是精准的情况下的一种判断情况，如果自动验证能够保证精准率高，是不是可以将验证机制合并到漏洞检测机制上，现有检测机制判断为漏洞后，接着就去验证漏洞，正常情况下一个漏洞已经判断为疑似存在的话，再请求下是否可获得为预期结果就能判断验证，最终再呈现是否真实存在漏洞，是不是好一些，剩余验证不了的或者验证没通过的可以定义为疑似漏洞
- 答：是的