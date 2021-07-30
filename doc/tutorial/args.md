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

### 1. 当需要自定义项目名称为 SpringDemo 时

```
java -javaagent:/path/to/agent.jar -Dproject.name=SpringDemo -jar SpringDemo.jar
```

### 2. 当需要本地调试 agent 或使用本地编译的 agent 时

```
java -javaagent:/path/to/agent.jar -Ddebug.name=true -jar SpringDemo.jar
```

### 3. 当需要设置 agent 延迟启动时间为15秒时

```
java -javaagent:/path/to/agent.jar -Diast.engine.delay.time=15 -jar SpringDemo.jar
```

### 4. 当需要在目录/tmp/class查看转换后的字节码文件时

```
java -javaagent:/path/to/agent.jar -Diast.dump.class.enable=true -Diast.dump.class.path=/tmp/class -jar SpringDemo.jar
```

### 5. 当需要设置HTTP代理为 10.100.100.1:80 时

```
java -javaagent:/path/to/agent.jar -Diast.proxy.enable=true -Diast.proxy.host=10.100.100.1 -Diast.proxy.host=80 -jar SpringDemo.jar
```

### 6. 当需要设置检测能力为hunter时

```
java -javaagent:/path/to/agent.jar -Diast.mode=hunter -jar SpringDemo.jar
```