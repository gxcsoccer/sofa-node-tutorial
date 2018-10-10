使用 sofa-node 来调用刚才发布的 rpc 服务

#### 1. 初始化服务提供方工程

`mkdir rpc-client && cd rpc-client && pwd`{{execute T2}}

`egg-init --type microservice`{{execute T2}}

#### 2. 引入接口定义

`rpc-client/proto/ProtoService.proto`{{open}}

#### 3. 配置 proxy

在 `$app_root/app/config/proxy.js` 里配置要调用的接口信息

<pre class="file" data-filename="rpc-client/config/proxy.js" data-target="replace">
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

#### 4. 配置服务地址

在 `rpc-client/config/config.default.js` 里配置服务提供者的地址（以 app 为单位）

<pre class="file" data-filename="rpc-client/config/config.default.js"  data-target="replace">
'use strict';

exports.sofaRpc = {
  client: {
    // 格式为：${appname}.rpc.service.url
    'sofarpc.rpc.service.url': '127.0.0.1:12200',
  },
};
</pre>

#### 5. 生成 proxy

`npm i && npm run init`{{execute T2}}

在 $app_root/app/proxy 下会生成 rpc 本地代理

#### 6. 调用服务

在 controller 里通过 ctx.proxy.xxx 来调用 rpc 方法

<pre class="file" data-filename="rpc-client/app/controller/home.js"  data-target="replace">
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

`npm run start -- --port 6001`{{execute T2}}

#### 6. 验证

`curl http://127.0.0.1:6001`{{execute T2}}

返回 `hello zongyu from sofa-node`

http://[[HOST_SUBDOMAIN]]-6001-[[KATACODA_HOST]].environments.katacoda.com/
