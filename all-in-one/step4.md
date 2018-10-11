Zookeeper 做注册中心实现软负载

#### 1. 从 docker 启动 zookeeper

`docker run -p 2181:2181  -d zookeeper:latest`{{execute T3}}


#### 2. 暂停服务

停掉 rpc-server
`npm run stop`{{execute T1}}

停掉 rpc-client
`npm run stop`{{execute T2}}

#### 3. 配置 zk 地址

修改客户端配置 `rpc-client/config/config.default.js`

<pre class="file" data-filename="rpc-client/config/config.default.js" data-target="replace">
'use strict';

exports.sofaRpc = {
  registry: {
    address: '127.0.0.1:2181',
  },
};
</pre>

修改服务器端配置 `rpc-server/config/config.default.js`

<pre class="file" data-filename="rpc-server/config/config.default.js" data-target="replace">
'use strict';

exports.sofaRpc = {
  registry: {
    address: '127.0.0.1:2181',
  },
  server: {
    namespace: 'com.alipay.sofa.rpc.protobuf'
  },
};
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
