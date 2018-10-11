SOFA-Node + Zipkin 实现分布式追踪

#### 1. 从 docker 启动 zipkin

`docker run -d -p 9411:9411  openzipkin/zipkin`{{execute T3}}

#### 2. 暂停服务

停掉 rpc-server

`npm run stop`{{execute T1}}

停掉 rpc-client

`npm run stop`{{execute T2}}

#### 3. 开启 egg-zipkin 插件

修改客户端配置 `rpc-client/config/plugin.js`

<pre class="file" data-filename="rpc-client/config/plugin.js" data-target="replace">
'use strict';

exports.zipkin = true;
</pre>

修改服务器端配置 `rpc-server/config/plugin.js`

<pre class="file" data-filename="rpc-server/config/plugin.js" data-target="replace">
'use strict';

exports.zipkin = true;
</pre>

#### 4. 重启服务

启动 rpc-server

`npm run start`{{execute T1}}

启动 rpc-client

`npm run start -- --port 6001`{{execute T2}}


#### 5. 验证

`curl http://127.0.0.1:6001`{{execute T2}}

返回 `hello zongyu from sofa-node`

http://[[HOST_SUBDOMAIN]]-6001-[[KATACODA_HOST]].environments.katacoda.com/

#### 6. 打开 zipkin 控制台

http://[[HOST_SUBDOMAIN]]-9411-[[KATACODA_HOST]].environments.katacoda.com/

可以搜索刚才的那个请求的追踪信息
