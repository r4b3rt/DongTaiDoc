# 探针参数

## 一、参数表

### debug

|   属性   | 值                                                           |
| :------: | :----------------------------------------------------------- |
| 生效方式 | 重启应用，启动应用时添加`-Ddebug=<true or false>`                           |
| 参数类型 | Boolean                                                      |
|   来源   | 命令行参数                                                   |
| 可选参数 | true｜false                                                  |
|  默认值  | false                                                        |
| 参数说明 | <div style="width: 300pt">开启后检测本地临时目录中是否存在核心检测引擎存在，加载本地检测引擎并启动 |

### project.name

|   属性   | 值                                        |
| :------: | :---------------------------------------- |
| 生效方式 | 重启应用，启动应用时添加`-Dproject.name=<Demo>` |
| 参数类型 | 字符串                                    |
|   来源   | 配置文件                                  |
| 可选参数 | 任意字符串                                |
|  默认值  | Demo Project                              |
| 参数说明 | <div style="width: 300pt">项目名称                                  |

### iast.mode

|   属性   | 值                                                           |
| :------: | :----------------------------------------------------------- |
| 生效方式 | <div style="width: 300pt">重启应用，启动应用时添加`-Diast.mode=<hunter or normal>`                       |
| 参数类型 | 字符串                                                       |
|   来源   | 配置文件                                                     |
| 可选参数 | hunter｜normal                                               |
|  默认值  | normal                                                      |
| 参数说明 | <div style="width: 300pt">漏洞检验模式，hunter模式漏洞多、误报率高，normal模式漏洞相对少、误报率低 |

### iast.server.mode

|   属性   | 值                                                           |
| :------: | :----------------------------------------------------------- |
| 生效方式 | <div style="width: 300pt">重启应用，启动应用时添加`-Diast.server.mode=<local or remote>`                |
| 参数类型 | 字符串                                                       |
|   来源   | 配置文件                                                     |
| 可选参数 | local｜remote                                                |
|  默认值  | remote                                                       |
| 参数说明 | <div style="width: 300pt">local模式支持单漏洞验证、项目漏洞批量验证、POST请求包展示、污点位置及污点值展示等功能 |

### iast.proxy.enable

|   属性   | 值                                             |
| :------: | :--------------------------------------------- |
| 生效方式 | <div style="width: 300pt">重启应用，启动应用时添加`-Diast.proxy.enable=<true or false>` |
| 参数类型 | Boolean                                        |
|   来源   | 配置文件                                       |
| 可选参数 | true｜false                                    |
|  默认值  | false                                          |
| 参数说明 | <div style="width: 300pt">HTTP代理模式是否启用                           |

### iast.proxy.host

|   属性   | 值                                           |
| :------: | :------------------------------------------- |
| 生效方式 | <div style="width: 300pt">重启应用，启动应用时添加`-Diast.proxy.host=<ip>` |
| 参数类型 | 字符串                                       |
|   来源   | 配置文件                                     |
| 可选参数 |                                              |
|  默认值  | false                                        |
| 参数说明 | <div style="width: 300pt">HTTP代理的域名(IP)                           |

### iast.proxy.port

|   属性   | 值                                           |
| :------: | :------------------------------------------- |
| 生效方式 | 重启应用，启动应用时添加`-Diast.proxy.port=<port>` |
| 参数类型 | 字符串                                       |
|   来源   | 配置文件                                     |
| 可选参数 |                                              |
|  默认值  | 80                                           |
| 参数说明 | <div style="width: 300pt">HTTP代理的端口                               |

### iast.service.report.interval

|   属性   | 值                                                        |
| :------: | :-------------------------------------------------------- |
| 生效方式 | <div style="width: 300pt">重启应用，启动应用时添加`-Diast.service.report.interval=<60000>` |
| 参数类型 | 整型数字                                                  |
|   来源   | 配置文件                                                  |
| 可选参数 | 任意整型数字                                              |
|  默认值  | 60000                                                     |
| 参数说明 | <div style="width: 300pt">发送报告的间隔时间，单位是毫秒                            |

### iast.service.replay.interval

|   属性   | 值                                                        |
| :------: | :-------------------------------------------------------- |
| 生效方式 | <div style="width: 300pt">重启应用，启动应用时添加`-Diast.service.replay.interval=<300000>` |
| 参数类型 | 整型数字                                                  |
|   来源   | 配置文件                                                  |
| 可选参数 | 任意整型数字                                              |
|  默认值  | 300000                                                    |
| 参数说明 | <div style="width: 300pt">重放的时间间隔，单位是毫秒                                |

### iast.engine.delay.time

|   属性   | 值                                                  |
| :------: | :-------------------------------------------------- |
| 生效方式 | 重启应用，启动应用时添加`-Diast.engine.delay.time=<10>` |
| 参数类型 | 整型数字                                            |
|   来源   | 配置文件                                            |
| 可选参数 | 任意整型数字                                        |
|  默认值  | 10                                                  |
| 参数说明 | <div style="width: 300pt">延迟启动功能，单位为秒                              |

### iast.dump.class.enable

|   属性   | 值                                                  |
| :------: | :-------------------------------------------------- |
| 生效方式 | <div style="width: 300pt">重启应用，启动应用时添加`-Diast.dump.class.enable=<true or false>` |
| 参数类型 | Boolean                                             |
|   来源   | 配置文件                                            |
| 可选参数 | true｜false                                         |
|  默认值  | false                                               |
| 参数说明 | <div style="width: 300pt">是否 dump 修改后的字节码                            |

### iast.dump.class.path

|   属性   | 值                    |
| :------: | :-------------------- |
| 生效方式 | 重启应用              |
| 参数类型 | 字符串                |
|   来源   | 配置文件              |
| 可选参数 | 任意有权限路径        |
|  默认值  | /tmp/iast-class-dump/ |
| 参数说明 | <div style="width: 300pt">dump 字节码的路径     |

### iast.server.url

|   属性   | 值                                  |
| :------: | :---------------------------------- |
| 生效方式 | 重启应用                            |
| 参数类型 | 字符串                              |
|   来源   | 配置文件                            |
| 可选参数 |                                     |
|  默认值  | http://openapi.iast.huoxian.cn:8000 |
| 参数说明 | <div style="width: 300pt">server url                          |

### iast.allhook.enable

|   属性   | 值       |
| :------: | :------- |
| 生效方式 | 重启应用 |
| 参数类型 | Boolean  |
|   来源   | 配置文件 |
| 可选参数 |          |
|  默认值  | false    |
| 参数说明 | <div style="width: 300pt">         |

## 二、用例

> 以测试项目 SpringDemo 为例。

### 1. 当需要将应用绑定到云端项目 SpringDemo 时：

```
java -javaagent:/path/to/agent.jar -Dproject.name=SpringDemo -jar SpringDemo.jar
```

### 2. 当需要排查 agent 报错问题或者二次开发 agent 时需要本地调试：

```
java -javaagent:/path/to/agent.jar -Ddebug.name=true -jar SpringDemo.jar
```

### 3. 当启动 agent 影响了应用的运行，需要设置 agent 延迟启动时间，以15秒为例：

```
java -javaagent:/path/to/agent.jar -Diast.engine.delay.time=15 -jar SpringDemo.jar
```

### 4. 当排查 agent 异常或者研究字节码转换原理时，在目录/tmp/class查看转换后的字节码文件：

```
java -javaagent:/path/to/agent.jar -Diast.dump.class.enable=true -Diast.dump.class.path=/tmp/class -jar SpringDemo.jar
```

### 5. 当前网络无法访问[洞态云端](https://iast.huoxian.cn)需要设置HTTP代理，以设置代理 10.100.100.1:80 为例：

```
java -javaagent:/path/to/agent.jar -Diast.proxy.enable=true -Diast.proxy.host=10.100.100.1 -Diast.proxy.host=80 -jar SpringDemo.jar
```

### 6. 当需要设置检测能力为 hunter/normal 时（hunter 模式的使用场景：代码审计，normal 模式使用场景：企业内部检测漏洞）：

```
java -javaagent:/path/to/agent.jar -Diast.mode=hunter/normal -jar SpringDemo.jar
```