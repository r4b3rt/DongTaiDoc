> "火线平台"与vulhub项目联合推出vulhub-cli工具，简化vulhub靶场的部署，可快速体验“火线～洞态IAST”

#### Vulhub靶场快速安装“火线～洞态IAST”

对于Vulhub靶场，其使用docker-compose进行管理，为了方便社区用户方便，直接对docker-compose进行封装，生成`vulhub-cli`工具，方便社区用户直接使用“火线～洞态IAST”。

[vulhub-cli官方文档](https://github.com/huoxianclub/vulhub-compose)

```shel
# 安装vulhub-cli
$ pip install vulhub-cli

# 使用vulhub-cli启动靶场环境并安装公共洞态IAST（漏洞数据所有人可见）
$ vulhub-cli remote start --app solr/ssrf_path --plugin dongtai

# 使用vulhub-cli启动靶场环境并安装个人洞态IAST（漏洞数据仅个人可见）
$ vulhub-cli remote start --app fastjson/1.2.24-rce --plugin dongtai --plugin-args "token=<dongtai iast token>"

# 使用vulhub-cli停止并销毁靶场
$ vulhub-cli remote stop --app fastjson/1.2.24-rce --plugin dongtai
```

“火线～洞态IAST”的agent与vulhub中的apache solr靶场的JDK存在兼容问题，暂时无法安装agent，可自行前往`apache solr`官网下载部署或联系[技术支持](/doc/aboutus/support)获取。