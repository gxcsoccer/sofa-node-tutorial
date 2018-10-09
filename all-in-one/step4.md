通过 zookeeper 实现服务发现

#### 1. 从 docker 启动 zookeeper

`docker run -p 2181:2181  -d zookeeper:latest`{{execute T3}}


#### 2. 暂停服务

`npm run stop`{execute T1}
`npm run stop`{execute T2}

#### 3. 配置 zk 地址

修改客户端配置 `sofa-rpc-client/config/config.default.js`

<pre class="file" data-filename="sofa-rpc-client/config/config.default.js" data-target="replace">
'use strict';

exports.sofaRpc = {
  registry: {
    address: '127.0.0.1:2181',
  },
};
</pre>

修改服务器端配置 `sofa-rpc-server/config/config.default.js`

<pre class="file" data-filename="sofa-rpc-server/config/config.default.js" data-target="replace">
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

`npm run start`{execute T1}
`npm run start`{execute T2}


#### 5. 验证

`curl http://127.0.0.1:7001`{{execute T2}}

返回 `hello zongyu from sofa-node`

http://[[CLIENT_SUBDOMAIN]]-7001-[[KATACODA_HOST]].environments.katacoda.com/
