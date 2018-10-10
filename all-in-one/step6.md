SOFA-Node + Prometheus 实现度量分析

#### 1. 从 docker 启动 prometheus

`echo "global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
alerting:
  alertmanagers:
  - static_configs:
    - targets:

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090', '172.18.0.1:3000']

    tls_config:
      insecure_skip_verify: true
" > ~/prometheus.yml`{{execute T3}}


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


添加 metrics

`sum(rate(http_request_rate[5m])) by (instance)`

`avg(http_response_time_ms{quantile="0.95"})`

`sum without (instance)(rate(http_response_time_ms{quantile="0.95"}[5m]))`
