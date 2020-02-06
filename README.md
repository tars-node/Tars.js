# Tars.js

With the development of the Internet, more and more services can be carried by not only a single node (or a single language), but also multi-language distributed collaborative development (such as the access layer is completed by Node.js, ) Layer is implemented by C ++ / GO / Python) and from this form a large heterogeneous system.

We developed Tars.js based on the [Tars](http://tars.tencent.com) system so that users can quickly build (migrate) Node.js services without changing the overall architecture of heterogeneous systems, and it is very convenient Split the original single service into multiple (logical) subservices.

![Tars.js](https://github.com/tars-node/Tars.js/blob/master/docs/images/tarsjs_architecture.png?raw=true)

Tars.js has been accumulated and iterated in Tencent for more than 5 years (supported by the Node.js@0.10 version), and is widely used in Tencent QQ browser, Tencent desktop browser, Tencent map, App treasure, Tencent mobile butler, Internet +, Tens Medical, Tencent Medical, insurance, lottery and other dozens of important businesses, Japan has undertaken tens of billions of traffic.

## Tars.js includes the following features:

* 100% written in JavaScript without any C / C ++ code.
* Multi-process load balancing and management.
* Code exception monitoring and restart.
* Collection and processing of service logs.
* HTTP (s) service monitoring and usage report automatically, and support user-defined dimension reporting (PP monitoring).
* Tars (IDL) codec module.
* Support Tars RPC calling and dyeing (modulo automatic reporting).
* Support online management command sending and pull service configuration.
* Original LongStackTrace ™ exception tracking mechanism.
* ...... More features can be found at [@ tars / node-agent] (https://www.npmjs.com/package/@tars/node-agent)

## design concept

### High degree of freedom

* Compatible with all (≥0.10) official Node.js versions.
* No intrusion or modification to Node.js source code.
* The bottom layer is completely transparent to the upper layer and supports a variety of upper-level frameworks, without changes.

In other words:

__You can use any framework you are familiar with (such as Express.js / Koa.js, including but not limited to web frameworks), and you don't need to modify the framework (without introducing any middleware).__ You can run it through Tars.js and enjoy the various monitoring and management features provided by the platform.

At the same time, the modules provided by Tars.js can also be imported according to your needs (or not imported if not used).

### High performance

Tars.js is designed for high performance and large concurrency, and uses a large number of front-end (V8) optimization techniques (such as FlattenString / FastProperties, etc.) to minimize the impact of the capabilities provided on business performance.

After our testing (Web Server), the default impact of bypass reporting and monitoring on service performance is ≤ 5%, and the performance of commonly used modules (RPC, logs, etc.) is among the best in the industry.

### Differentiation

Tars.js provides differentiated operation solutions according to different business types:
* High-traffic services: Try to reduce the impact of the framework on business performance.
* Low-traffic business: make full use of hardware resources to improve development experience.

## List of available modules

Due to the limited space, I can't show all the capabilities. If you have more requirements (such as RPC calls, etc.), you can use the modules provided by Tars.js (as follows):

* [@ tars/stream](https://www.npmjs.com/package/@tars/stream): Tars (Tup) protocol codec module can be combined with [tars2node](https://github.com/tars-node).
* [@ tars/rpc](https://www.npmjs.com/package/@tars/rpc): Tars RPC calling module.
* [@ tars/logs](https://www.npmjs.com/package/@tars/logs): Log component, including rolling (by size, time) and remote logs.
* [@ tars/config](https://www.npmjs.com/package/@tars/config): Used to obtain service configuration files online.
* [@ tars/monitor](https://www.npmjs.com/package/@tars/monitor): Provides support for service monitoring, feature monitoring, and PP monitoring reporting.
* [@ tars/notify](https://www.npmjs.com/package/@tars/notify): Used to report service (alarm) messages.
* [@ tars/utils](https://www.npmjs.com/package/@tars/utils): A collection of auxiliary tools, including Tars configuration files and Tars RPC Endpoint parser.
* [@ tars/dyeing](https://www.npmjs.com/package/@tars/dyeing): Tars RPC dyeing definition module.
* [@ tars/registry](https://www.npmjs.com/package/@tars/registry): Used for Tars name service query (Servant ===> Endpoint).

Each module (click the name to jump) has extremely detailed documentation (README) for you to consult at any time.

## Hello World

1. On the Tars platform-> Service Management-> Service Go Online, launch a new service:
* Basic service information:
* Service Name: HelloWorld
* Service Type: NODEJS
* Template name: tars.cloud.default
* OBJ deployment information:
* OBJ name: HttpObj
* Yes TARS: Select No (remove check box)
* Port: Automatically generated port

2. Install the [@ tars/deploy](https://www.npmjs.com/package/@tars/deploy) packaging tool.
```bash
npm i -g @ tars / deploy
```

3. Choose the framework based on your business needs and your preferences (!Tars.js does not limit the framework you use!), Write your business code:
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
The above is an example of [HTTP Server](https://nodejs.org/en/about/) on the official website of Node.js. It only needs to modify the IP and port to run on the `Tars.js` platform.
The IP and PORT obtained in the environment variables here are the IP and port in the OBJ deployment information you configured in the first step.

4. Execute the packaging tool in the project root directory to generate the release package HelloWorld.tgz:
```bash
tars-deploy HelloWorld
```

5. On the Tars platform, select the service you just created-> Release Management-> Manually upload the release package, and then publish this version.

6. You can access your services through the IP and port you configured in the first step.
