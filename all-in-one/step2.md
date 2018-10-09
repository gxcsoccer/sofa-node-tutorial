初始化服务提供方工程

`mkdir rpc-server && cd rpc-server`{{execute T1}}

`egg-init`{{execute T1}}

`npm i`{{execute T1}}

打开 `rpc-server/config/plugin.js`{{open}}

<pre class="file" data-filename="rpc-server/config/plugin.js" data-target="replace">
'use strict';

exports.sofaRpc = {
	enable: true,
	package: 'egg-sofa-rpc',
};
</pre>

定义接口 

`mkdir proto && touch rpc-server/proto/ProtoService.proto`{{execute T1}}

<pre class="file" data-filename="rpc-server/proto/ProtoService.proto" data-target="replace">
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

打开 `rpc-server/config/config.default.js`{{open}}

<pre class="file" data-filename="rpc-server/config/plugin.js" data-target="clipboard">
config.sofaRpc = {
  registry: {
    address: '127.0.0.1:2181',
  },
	server: {
    namespace: 'com.alipay.sofa.rpc.protobuf'
  },
};
</pre>

实现服务逻辑

`mkdir rpc-server/app/rpc && touch rpc-server/app/rpc/ProtoService.js`{{execute T1}}

<pre class="file" data-filename="rpc-server/app/rpc/ProtoService.js" data-target="replace">
'use strict';

exports.echoObj = async function(req) {
  return {
    code: 200,
    message: 'hello ' + req.name + ' from sofa-node',
  };
};

exports.interfaceName = 'com.alipay.sofa.rpc.protobuf.ProtoService';
</pre>

`npm i egg-rpc-generator --save-dev`{{execute T1}}
