使用 sofa-node 来调用刚才发布的 rpc 服务

#### 1. 初始化服务提供方工程

`cd sofa-rpc-client`{{execute T2}}

`pwd`{{execute T2}}

#### 2. 引入接口定义

`mkdir proto && cp ../sofa-rpc-server/proto/ProtoService.proto proto/ProtoService.proto`{{execute T2}}

#### 3. 配置 proxy

在 `$app_root/app/config/proxy.js` 里配置要调用的接口信息

<pre class="file" data-filename="sofa-rpc-client/config/proxy.js" data-target="replace">
'use strict';

module.exports = {
  services: [{
    appName: 'sofarpc',
    api: {
      ProtoService: 'com.alipay.sofa.rpc.protobuf.ProtoService',
    },
  }],
};
</pre>

#### 5. 配置服务地址

在 `sofa-rpc-client/config/config.default.js` 里配置服务提供者的地址（以 app 为单位）

<pre class="file" data-filename="sofa-rpc-client/config/config.default.js"  data-target="replace">
'use strict';

exports.sofaRpc = {
  client: {
    'sofarpc.rpc.service.url': '127.0.0.1:12200',
  },
};
</pre>

#### 6. 生成 proxy

`npm i && npm run rpc`{{execute T2}}

在 $app_root/app/proxy 下会生成 rpc 本地代理

#### 7. 调用服务

在 controller 里通过 ctx.proxy.xxx 来调用 rpc 方法

<pre class="file" data-filename="sofa-rpc-client/app/controller/home.js"  data-target="replace">
'use strict';

const Controller = require('sofa-node-demo').Controller;

class HomeController extends Controller {
  async index() {
    const ctx = this.ctx;
    const data = await ctx.proxy.protoService.echoObj({
      name: 'zongyu',
      group: 'B',
    });
    ctx.body = data.message;
  }
}

module.exports = HomeController;
</pre>

#### 5. 启动应用

`npm run start`{{execute T2}}

#### 6. 验证

`curl http://127.0.0.1:7001`{{execute T2}}

返回 `hello zongyu from sofa-node`

http://[[CLIENT_SUBDOMAIN]]-7001-[[KATACODA_HOST]].environments.katacoda.com/
