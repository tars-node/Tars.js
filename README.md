# Tars.js

随着互联网的发展，越来越多的业务不仅仅由单一节点（或是单一语言）就可承载，而是趋向多语言分布式协同开发（如接入层由 Node.js 完成，逻辑（数据）层由 C++/GO/Python 实现）并由此组成大型异构系统。

我们基于 [Tars](http://tars.tencent.com) 体系研发出 Tars.js 以便用户在不改变异构系统整体架构的情况下快速搭建（迁移） Node.js 服务，并可非常方便的将原来的单一服务拆分为多个（逻辑）子服务。

![Tars.js](https://github.com/tars-node/Tars.js/blob/master/docs/images/tarsjs_architecture.png?raw=true)

Tars.js 在腾讯内部经过 5 年多的沉淀与迭代（Node.js@0.10版本即提供支持），广泛运用于腾讯QQ浏览器、腾讯桌面浏览器、腾讯地图、应用宝、腾讯手机管家、互联网+、腾讯医疗、腾讯觅影、保险、彩票等几十个重要业务中，日承担了上百亿流量。

## Tars.js 包含下述特性：

* 100% 由 JavaScript 编写，不包含任何 C/C++ 代码。
* 多进程负载均衡与管理。
* 代码异常监控与重启。
* 服务日志搜集与处理。
* HTTP(s) 服务监控与用量自动上报，并支持用户自定义维度上报（PP 监控）。
* 符合 Tars(IDL) 规范的编解码模块。
* 支持 Tars RPC 调用与染色（模调自动上报）。
* 支持在线发送管理命令、拉取服务配置。
* 独创 LongStackTrace™ 异常跟踪机制。
* …… 更多特性可访问 [@tars/node-agent](https://www.npmjs.com/package/@tars/node-agent) 了解

## 设计理念

### 高自由度

* 兼容所有（≥0.10）官方 Node.js 版本。
* 对 Node.js 源码无侵入无修改。
* 底层对上层完全透明，支持各种上层框架，无需变更。

也就是说：

__您可以使用任何您熟悉的框架（如 Express.js / Koa.js 等，包括但不仅限于 Web 框架），也无需对框架进行任何修改（无需引入任何中间件）。__ 即可通过 Tars.js 运行，享受平台提供的各种监控与管理特性。

与此同时，Tars.js 所提供的模块，也可以根据您的需求引入（如未使用到则可不引入）。

### 高性能

Tars.js 为高性能与大并发量而设计，使用了大量的前端（V8）优化技巧（如 FlattenString/FastProperties 等）尽量降低所提供的能力对于业务性能的影响。

经过我们测试（Web Server），默认的旁路上报与监控对服务性能的影响≤ 5%，常用模块（RPC、日志等）性能位于业界前列。

### 差异化

Tars.js 根据不同的业务类型提供差异化运营方案：
* 高流量业务：尽力降低框架对业务性能的影响。
* 低流量业务：充分利用硬件资源提升开发体验。

## 可用模块列表

由于篇幅有限未能展示所有能力，如果您有更多需求（如 RPC 调用等）可使用 Tars.js 所提供的模块（如下）实现：

* [@tars/stream](https://www.npmjs.com/package/@tars/stream) : Tars(Tup) 协议编解码模块，可结合 [tars2node](https://github.com/tars-node/tars2node) 使用。
* [@tars/rpc](https://www.npmjs.com/package/@tars/rpc) : Tars RPC 调用模块。
* [@tars/logs](https://www.npmjs.com/package/@tars/logs)：日志组件，包含（按大小、时间）滚动与远程日志。
* [@tars/config](https://www.npmjs.com/package/@tars/config)：用于在线获取服务配置文件。
* [@tars/monitor](https://www.npmjs.com/package/@tars/monitor)：提供服务监控、特性监控与PP监控上报支持。
* [@tars/notify](https://www.npmjs.com/package/@tars/notify)：用于服务（告警）消息上报。
* [@tars/utils](https://www.npmjs.com/package/@tars/utils)：辅助工具集合，包含 Tars 配置文件与 Tars RPC Endpoint 解析器。
* [@tars/dyeing](https://www.npmjs.com/package/@tars/dyeing)：Tars RPC 染色定义模块。
* [@tars/registry](https://www.npmjs.com/package/@tars/registry)：用于 Tars 名字服务查询（Servant ===> Endpoint）。

每个模块（点击名称跳转）均有极为详细的文档（README）方便您在任何时候查阅。

## Hello World

1. 在 Tars 平台 -> 服务管理 -> 服务上线，上线一个新服务：
	* 服务基本信息：
		* 服务名称：HelloWorld
		* 服务类型：NODEJS
		* 模板名称：tars.cloud.default
	* OBJ 部署信息：
		* OBJ名称：HttpObj
		* 是TARS：选择否（移除复选框）
		* 端口：自动生成端口

2. 安装 [@tars/deploy](https://www.npmjs.com/package/@tars/deploy) 打包工具。
``` bash
npm i -g @tars/deploy
```

3. 根据业务需求以及您的喜好选择框架（！Tars.js 不限制您使用的框架！），编写业务代码：
``` js
const http = require('http');

const hostname = process.env.IP || '127.0.0.1';
const port = process.env.PORT || 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
上述为 Node.js 官网 [HTTP Server](https://nodejs.org/en/about/) 例子，在 `Tars.js` 平台上运行只需要修改 IP 和端口即可。
这里在环境变量中获得到的 IP 和 PORT 即为您在第一步配置的 OBJ 部署信息中的 IP 与端口。

4. 在项目根目录下执行打包工具，即可生成发布包 HelloWorld.tgz：
``` bash
tars-deploy HelloWorld
```

5. 在 Tars 平台上选择您刚刚创建的服务 -> 发布管理 -> 手动上传发布包，然后发布此版本即可。

6. 通过您在第一步配置的 IP 与 端口即可访问您的服务。