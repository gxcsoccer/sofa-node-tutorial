使用 sofa-node 来发布 rpc 服务

#### 1. 初始化服务提供方工程

`mkdir rpc-server && cd rpc-server && pwd`{{execute T1}}

`egg-init --type microservice`{{execute T1}}

#### 2. 配置 sofaRpc server

打开 `rpc-server/config/config.default.js`

<pre class="file" data-filename="rpc-server/config/config.default.js"  data-target="replace">
'use strict';

exports.sofaRpc = {
  server: {
    namespace: 'com.alipay.sofa.rpc.protobuf'
  },
};
</pre>

#### 3. 定义接口

`rpc-server/proto/ProtoService.proto`{{open}}

#### 4. 实现接口逻辑

在 $app_root/app/rpc 目录下创建一个和服务同名的 js 文件

`mkdir app/rpc && touch app/rpc/ProtoService.js`{{execute T1}}

实现具体的接口业务

<pre class="file" data-filename="rpc-server/app/rpc/ProtoService.js" data-target="replace">
'use strict';

exports.echoObj = async function(req) {
  return {
    code: 200,
    message: 'hello ' + req.name + ' from sofa-node',
  };
};
</pre>

#### 5. 启动应用

`npm i && npm run start`{{execute T1}}

#### 6. 验证

`netstat -an | grep 12200`{{execute T1}}

```bash
tcp6       0      0 :::12200                :::*                    LISTEN
```
