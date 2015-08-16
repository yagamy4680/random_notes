[[_TOC_]]

### [7 Minimal Node.js Web Frameworks for 2014 and Beyond](http://codecondo.com/7-minimal-node-js-web-frameworks/)
- Express – node.js web application framework
- Flatiron, A framework for Node.js
- Koa – next generation web framework for node.js
- total.js / web framework for node.js
- restify – building REST APis with Node.js

### Articles

- [Finding the right Node.js WebSocket implementation - A journey with ws, Socket.IO, Engine.IO, and Primus](https://medium.com/node-js-javascript/b63bfca0539)
- [Seven Things You Should Stop Doing with Node.js](http://webapplog.com/seven-things-you-should-stop-doing-with-node-js/)
  - Stop using callbacks
  - Stop using * for versions
  - Stop using console.log for debugging
  - Stop using GET and POST for everything
  - Stop using semicolons
  - Stop using comma-first style
  - Stop limiting your connections with default maxSockets value
- [Gulp + Browserify: The Everything Post](http://viget.com/extend/gulp-browserify-starter-faq)

### Nodejs for TinyCoreLinux

[tiny-node](https://github.com/sihorton/tiny-node) nodejs extension for TinyCoreLinux.

Built successfully on CorePlus-4.7.3.iso tcz extensions: 

```shell
tce-load -i -w git 
tce-load -i -w curl 
tce-load -i -w expat2 

git clone http://github.com/sihorton/tiny-node.git

tce-load -i -w compiletc 
tce-load -i -w make 
tce-load -i -w python 
tce-load -i -w openssl-1.0.0.tcz 
tce-load -i -w openssl-1.0.0-dev.tcz 
tce-load -i -w squashfs-tools-4.x.tcz

git clone https://github.com/joyent/node.git src
cd tiny-node
sudo ./make v0.8.18
```

### Desktop Notifications

Status and error messages can be displayed as desktop notification using either Growl or libnotify.

![](https://github-camo.global.ssl.fastly.net/7adb738e7a64557d92556307614cce7bad510f39/687474703a2f2f66676e6173732e6769746875622e636f6d2f696d616765732f6e6f64652d6465762e706e67)

#### [nw-desktop-notifications](https://github.com/robrighter/nw-desktop-notifications), Simple cross platform desktop notifications for node-passbook

```javascript
window.LOCAL_NW.desktopNotifications.notify(iconUrl, title, content, clickHandlerCallback);
```

BTW, also need to read the [Github issue](https://github.com/rogerwang/node-webkit/issues/27) talking about desktop notification feature for Node-Webkit.



### Modules

#### [Hipache](https://github.com/dotcloud/hipache), a distributed HTTP and websocket proxy

Hipache is a distributed proxy designed to route high volumes of http and websocket traffic to unusually large numbers of virtual hosts, in a highly dynamic topology where backends are added and removed several times per second. It is particularly well-suited for PaaS (platform-as-a-service) and other environments that are both business-critical and multi-tenant.

#### [node-opencv](https://github.com/peterbraden/node-opencv), OpenCV bindings for Node.js

OpenCV is the defacto computer vision library - by interfacing with it natively in node, we get powerful real time vision in js.

(yagamy: now it supports opencv 2.3.1)


#### [node-dev](https://github.com/fgnass/node-dev), process management for nodejs apps

Node-dev is a development tool for Node.js that automatically restarts the node process when a script is modified.

#### [Prerender.io](https://prerender.io/), allow your Javascript apps to be crawled perfectly by search engines. (Not-free solution!!!)


#### [Phaser](https://github.com/photonstorm/phaser) a fast, free and fun open source game framework for making desktop and mobile browser HTML5 games.

![phaser](https://github-camo.global.ssl.fastly.net/cddfd61e5f61e95a242e210c7f56471f0eed9ecf/687474703a2f2f7777772e70686f746f6e73746f726d2e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031332f30392f7068617365725f31305f72656c656173652e6a7067)


#### [node-llvm](https://github.com/kevinmehall/node-llvm), LLVM bindings for Node.JS.
Requires LLVM 3.2:
`sudo apt-get install libllvm3.2 llvm-3.2-dev`

Features
- Wraps the most important LLVM APIs.
- A port of the LLVM Kaleidoscope example can be found in examples/. Expressions must be entered on a single line.
- Uses [Node-FFI](https://github.com/rbranson/node-ffi/wiki/Node-FFI-Tutorial) to make JIT functions callable from JS.
- Currently does not free LLVM objects' memory when a Module is GC'd.

#### [node-ffi](https://github.com/rbranson/node-ffi), invoke C/C++ functions directly from nodejs
a powerful set of tools for interfacing with dynamic libraries using pure JavaScript in the Node.js environment. It can be used to build interface bindings for libraries without using any C++ code.

#### [VerbalExpressions](https://github.com/jehna/VerbalExpressions), a JavaScript library that helps to construct difficult regular expressions

#### [ConvNetJS](http://cs.stanford.edu/people/karpathy/convnetjs/), Deep Learning in your browser
a Javascript library for training Deep Learning models (mainly Neural Networks) entirely in your browser. Open a tab and you're training. No software requirements, no compilers, no installations, no GPUs, no sweat.


#### [Bucky](http://github.hubspot.com/bucky/), Open Source Real User Monitoring.
Open-source tool to measure the performance of your web app directly from your users' browsers.
![](http://github.hubspot.com/bucky/images/values.png)

#### [vis.js](http://visjs.org/#gallery)
Vis.js is a dynamic, browser based visualization library
![](http://visjs.org/img/gallery/timeline/01_basic.png)
![](http://visjs.org/img/gallery/graph/12_scalable_images.png)

#### [JsNice](http://www.jsnice.org/), statistical renaming, type inference and deobfuscation

#### [Crossbar.io](http://crossbar.io/howitworks/), an application router implemeting WAMP

Crossbar.io is an application router which implements the [Web Application Messaging Protocol (WAMP)](http://wamp.ws/). WAMP provides asynchronous Remote Procedure Calls and Publish & Subscribe (with WebSocket being one transport option) and allows to connect application components in distributed systems:

#### [convnetjs](https://github.com/karpathy/convnetjs), Deep Learning in Javascript

ConvNetJS implements Deep Learning models and learning algorithms as well as nice browser-based demos, all in Javascript.

#### [tracking.js](http://trackingjs.com/), A modern approach for Computer Vision on the web

The tracking.js library brings different computer vision algorithms and techniques into the browser environment. By using modern HTML5 specifications, we enable you to do real-time color tracking, face detection and much more — all that with a lightweight core (~7 KB) and intuitive interface.

#### [Basil.js](http://wisembly.github.io/basil.js/), Unified localstorage, cookie and session storage JavaScript API.

Basic Usage:

```javascript
basil = new window.Basil(options);

// basic methods
basil.set('foo', 'bar'); // store 'bar' value under 'foo' key
basil.get('foo'); // returns 'bar'
basil.remove('foo'); // remove 'foo' value

// advanced methods
basil.check('local'); // boolean. Test if localStorage is available
basil.reset(); // reset all stored values under namespace for current storage
```
#### [Sails.js](http://sailsjs.org/#/), designed to emulate the familiar MVC pattern of frameworks like Ruby on Rails

Sails makes it easy to build custom, enterprise-grade Node.js apps. It is designed to emulate the familiar MVC pattern of frameworks like Ruby on Rails, but with support for the requirements of modern apps: data-driven APIs with a scalable, service-oriented architecture. It's especially good for building chat, realtime dashboards, or multiplayer games; but you can use it for any web application project - top to bottom.

#### [Nigthmare.js](http://www.nightmarejs.org/), A high level wrapper for Phantomjs.

RAW PHANTOMJS

```javascript
phantom.create(function (ph) {
  ph.createPage(function (page) {
    page.open('http://yahoo.com', function (status) {
      page.evaluate(function () {
        var el =
          document.querySelector('input[title="Search"]');
        el.value = 'github nightmare';
      }, function (result) {
        page.evaluate(function () {
          var el = document.querySelector('.searchsubmit');
          var event = document.createEvent('MouseEvent');
          event.initEvent('click', true, false);
          el.dispatchEvent(event);
        }, function (result) {
          ph.exit();
        });
      });
    });
  });
});
```

WITH NIGHTMARE

```javascript
new Nightmare()
  .goto('http://yahoo.com')
  .type('input[title="Search"]', 'github nightmare')
  .click('.searchsubmit')
  .run();
```

#### [mjpegcanvasjs](https://github.com/WPI-RAIL/mjpegcanvasjs), Display a MJPEG stream from the ROS mjpeg_server Inside of a HTML5 Canvas.

#### [jungledb](https://github.com/1602/jugglingdb), Multi-database ORM: redis, mongodb, mysql, sqlite, postgres, neo4j, memory...

#### [nan](https://github.com/rvagg/nan), Native Abstractions for Node.js

A header file filled with macro and utility goodness for making add-on development for Node.js easier across versions 0.8, 0.10 and 0.11, and eventually 0.12.[

#### [engine.io](https://github.com/LearnBoost/engine.io)
The main goal of Engine is ensuring the most reliable realtime communication. Unlike the previous Socket.IO core, it always establishes a long-polling connection first, then tries to upgrade to better transports that are "tested" on the side.

#### [javascript-load-image](https://github.com/blueimp/JavaScript-Load-Image), JavaScript Load Image is a library to load images

`JavaScript-Load-Image` loads images to provided as File or Blob objects or via URL. It returns an optionally scaled and/or cropped HTML img or canvas element.

#### [bluebird](https://github.com/petkaantonov/bluebird), a fully featured promise library with focus on innovative features and performance
![](https://raw.githubusercontent.com/petkaantonov/bluebird/master/logo.png)

#### [darkmagic](https://github.com/kessler/darkmagic), an experimental opinionated dependency injection module

This framework uses a lot of "dark magic" (hence its name) tricks that many will view as dangerous. These people are probably right and you should listen to them!

This module:

- parses function signature and uses the parameters, literally to load modules, first attempting to require them as they are and then by attaching them to various predefined search paths in your local file system

- Attempt to inject and invoke recursively EVERY module that exports a function and override the module system cache with the result of the invocation for that module. This behavior is customizable and is turned off by default for external modules (core/node_modules)

- dashify camelCase (camel-case) parameters when trying to find non local node modules

- infer that an exported function is async if the last paramter is called "callback"

- relies heavily on the module system, it does not cache the dependencies you create ** as a result, one injector is use for one process. You can create more injectors but they will share the same underlying require cache.

#### [Papa Parse](http://papaparse.com/docs.html#results), The powerful, in-browser CSV parser for big boys and girls

#### [jeieba](https://github.com/fxsjy/jieba), 结巴中文分词
"结巴"中文分词：做最好的 Python 中文分词组件 "Jieba" (Chinese for "to stutter") Chinese text segmentation: built to be the best Python Chinese word segmentation module.

#### [dnsd](https://github.com/jhs/dnsd), Dynamic authoritative name server

#### [electron](https://github.com/atom/electron), Build cross platform desktop apps with web technologies, (an alternative to node-webkit, from GitHub Atom project)

#### [Socket.IO P2P](http://socket.io/blog/socket.io-p2p/), provides an easy and reliable way to setup a WebRTC connection between peers and communicate using the socket.io-protocol.

![](https://cldup.com/95U80xyuHq.svg)

#### [Schyntax](https://github.com/schyntax/schyntax), a domain-specific language for defining event schedules in a terse, but readable, format.

![](https://avatars1.githubusercontent.com/u/8682785?v=2&s=200)

- [Schyntax Part 1: The Language](http://bret.codes/schyntax-part-1/)
- [Schyntax Part 2: The Task Runner](http://bret.codes/schyntax-part-2/)
- [js-schtick](https://github.com/schyntax/js-schtick), A Node.js scheduled task runner built on top of schyntax.

```js
var Schtick = require('schtick');

var schtick = new Schtick(); // best practice is to keep a singleton Schtick instance

schtick.addTask('unique task name', 'minutes(*)', function (task, eventTime) {
  console.log(task.name + ' ' + eventTime);
});
```



#### Bootstrap templates

- http://startbootstrap.com/popular-templates
- http://startbootstrap.com/modern-business
- http://startbootstrap.com/templates/modern-business/index.html
- https://github.com/DinisCruz/DocPad-modern-business

### Articles

[Better local require() paths for Node.js](https://gist.github.com/branneman/8048520)

1. The Symlink
2. The Global
3. The Module

[10 steps to nodejs nirvana in production](http://qzaidi.github.io/2013/05/14/node-in-production/)

1. Use upstart
2. Use cluster for multi-core environments
3. Use (re)cluster for zero downtime deployments
4. Heartbeat Checks
5. Automated deployments with deploy.sh
6. Set listen backlog, max agents and max open files limit.
7. Redis with hiredis.
8. Profile often, with the v8 profiler.
9. Enable GOD Mode with REPL

[Production Practices](http://www.joyent.com/developers/node/deploy), from Joyent

1. Cluster
2. Domains ([Domain](http://nodejs.org/api/domain.html) is a useful module built into the core of Node that allows developers to gracefully handle unexpected errors for asynchronous events.)
3. Deploying New Versions
4. Service Management
5. Dependency Management

[Best Practices for Deploying Node.js in Production](http://strongloop.com/strongblog/node-js-deploy-production-best-practice/)

1. Build your dependencies into your deployable packages
2. Be able to push deploys
3. Deploy and run your app inside a supervisor/manager
4. Use composable tools

[Error Handling in Node.js](https://www.joyent.com/developers/node/design/errors)

1. **Background**: what you're expected to know already.
2. **Operational errors vs. programmer errors**: introduction to two fundamentally different kinds of errors
3. **Patterns for writing functions**: general principles for writing functions that produce useful errors
4. **Specific recommendations for writing new functions**: a checklist of specific guidelines for writing robust functions that produce useful errors
5. **An example**: example documentation and preamble for a connect function
6. **Summary**: a summary of everything up to this point
7. **Appendix**: Conventional properties for Error objects: a list of property names to use for providing extra information in a standard way