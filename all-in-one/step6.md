SOFA-Node + Prometheus 实现度量分析

#### 1. 从 docker 启动 prometheus

`docker run -d -p 9090:9090 -v ~/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus`{{execute T3}}

#### 2. 暂停服务

`npm run stop`{{execute T2}}

#### 3. 开启 egg-prometheus 插件

修改客户端配置 `rpc-client/config/plugin.js`

<pre class="file" data-filename="rpc-client/config/plugin.js" data-target="replace">
'use strict';

exports.zipkin = true;
exports.prometheus = true;
</pre>

#### 4. 重启服务

`npm run start -- --port 6001`{{execute T2}}


#### 5. 验证

`curl http://127.0.0.1:6001`{{execute T2}}

返回 `hello zongyu from sofa-node`

http://[[HOST_SUBDOMAIN]]-6001-[[KATACODA_HOST]].environments.katacoda.com/

#### 6. 打开 prometheus 控制台

http://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/
