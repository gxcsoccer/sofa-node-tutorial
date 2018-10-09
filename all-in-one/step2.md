使用 sofa-node 来发布 rpc 服务

#### 1. 初始化服务提供方工程

`cd sofa-rpc-server`{{execute T1}}

#### 2. 配置 sofaRpc server

打开 `sofa-rpc-server/config/config.default.js`

<pre class="file" data-filename="sofa-rpc-server/config/config.default.js"  data-target="replace">
'use strict';

exports.sofaRpc = {
  server: {
    namespace: 'com.alipay.sofa.rpc.protobuf'
  },
};
</pre>

#### 3. 定义接口

`mkdir proto && touch proto/ProtoService.proto`{{execute T1}}

<pre class="file" data-filename="sofa-rpc-server/proto/ProtoService.proto" data-target="replace">
syntax = "proto3";

package com.alipay.sofa.rpc.protobuf;
option java_multiple_files = true; // 可选
option java_outer_classname = "ProtoServiceModels"; // 可选

service ProtoService {
    rpc echoObj (EchoRequest) returns (EchoResponse) {}
}

message EchoRequest {
    string name = 1;
    Group group = 2;
}

message EchoResponse {
    int32 code = 1;
    string message = 2;
}

enum Group {
    A = 0;
    B = 1;
}
</pre>

#### 4. 实现接口逻辑

在 $app_root/app/rpc 目录下创建一个和服务同名的 js 文件

`mkdir app/rpc && touch app/rpc/ProtoService.js`{{execute T1}}

实现具体的接口业务

<pre class="file" data-filename="sofa-rpc-server/app/rpc/ProtoService.js" data-target="replace">
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

`curl http://127.0.0.1:6001`{{execute T1}}

返回 `hi, egg`

`netstat -an | grep 12200`{{execute T1}}
