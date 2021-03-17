> 为了快速体验IAST的漏洞检测效果，我们将逐步提供不同类型的测试用例，用于更快的进行测试。

| 漏洞类型 | docker地址 |
|  ----  | ----  |
| 命令执行/路径穿越/OGNL代码执行 | owef/iast-demo01:v3 |

#### iast-demo01快速安装
1.安装并启动docker

2.拉取最新的容器：`docker pull owef/iast-demo01:v3`

3.启动容器：`docker run -itd -p 8080:8080 --name iast-demo01 owef/iast-demo01:v3`

4.访问靶场地址：
- [命令执行](http://localhost:8080/iast-test01/cmd.jsp?cmd=id)
- [OGNL代码执行](http://localhost:8080/iast-test01/ognl.jsp?exp=T(java.lang.Runtime).getRuntime().exec(%27whoami%27))
- [SQL注入](http://localhost:8080/iast-test01/sql.jsp?page=1&page_size=10)

#### Vulhub靶场快速安装灵芝IAST
对于Vulhub靶场，其使用docker-compose进行管理，为了方便社区用户方便，直接对docker-compose进行封装，生成`vulhub-cli`工具，方便社区用户直接使用灵芝IAST。
```shel
$ pip install vulhub-cli

$ vulhub-cli remote start --app fastjson/1.2.24-rce --plugin lingzhi

$ vulhub-cli remote stop --app fastjson/1.2.24-rce --plugin lingzhi
```
