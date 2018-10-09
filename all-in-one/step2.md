初始化服务提供方工程

`mkdir rpc-server && cd rpc-server`{{execute T1}}

`egg-init`{{execute T1}}

`npm i`{{execute T1}}

<pre class="file" data-filename="rpc-server/config/plugin.js" data-target="replace">
'use strict';

exports.sofaRpc = {
	enable: true,
	package: 'egg-sofa-rpc',
};
</pre>
