Yes, just my random notes, for any interesting topic, idea, and random thought at random time.

![](images/news1_1.jpg)

[[_TOC_]]

## Misc

- [[System Administration Notes|sys-administration-notes]]

- [[Arduino Resources|arduino]]

- [[ARM BareMetal Development Resources|arm-bare-metal]]

- [[BeagleBoneBlack Boot Process|bbb-boot-process]]

- [[Big Data Study Resources|big-data-study-resources]]

- [[Bluetooth - Linux Kernel Configs|bluetooth-linux-kernel-config]]

- [[Cubieboard - Linux Kernel Compilation Tips|cubieboard-linux-kernel-tips]]

- [[Docker Resources|docker-resources]]

- [[FrontEnd JavaScript Tips|frontend-js-tips]]
  - server-side device detection

- [[iBeacon Resources|ibeacon-resources]]

- [[Image Processing Resources|image-processing-resources]]

- [[Linux Webcam Tips|linux-webcam-tips]]

- [[LLVM Resources|llvm-resources]]

- [[Hypervisor Resources|hypervisor-resources]], e.g. qemu for rpi.

- [[Node.JS Resources|nodejs-resources]]

- [[OpenCL Resources|opencl-resources]]
- [[OpenCV Resources|opencv-resources]]
- [[OpenWRT|openwrt]]

- [[Time-Series DB|time-series-db]]

- [[Vagrant|vagrant-notes]]

- [[Embedded System Resources|embedded-system-resources]]

- [[Embedded Linux|embedded-linux]]

- [[Linux|linux]]

- [[NodeJS Study - SockServer for domain socket|exp-nodejs-socksrv-unixsock]]

- [[RTOS Resources|rtos-resources]]

- [[Compiler|compiler]]

- [[Management|management]]

- [[Functional Programming|functional-programming]]

## Idea

- [[cvapp design|idea-cvapp-design]]
- [[SeaDragon app design|idea-seadragon-app]]

[WakaTime](https://wakatime.com/), Fully automatic time tracking for programmers.


如果要打造"物聯網工程公司", 聘僱很多工程師, 也可以拿來那些 open-source 的 plugin 來改寫, 改送資料到自己的 server. (E.g. [sublime plugin](https://github.com/wakatime/sublime-wakatime), written in python). => 很快的查一下 codes, 發現所有 log 資料的會放在 `~/.wakatime.log`, JSON 格式, 可以很快的自行修改.. (refer to [log.py](https://github.com/wakatime/sublime-wakatime/blob/master/packages/wakatime/wakatime/log.py))


## Experiments

- [[distcc performance|exp-distcc]]
- [[Docker Private Registry|exp-private-docker-registry]]


## Programming Language Tips

- [[C++ Tips|cpp-tips]]
- [[Python Tips|python-tips]]


## Software-Defined-Everything

- [[SDN (Software-Defined-Network) articles|sdn-articles]]

## Civic Hacking

- [[clkao 環球考察分享會|clkao-around-the-world-2014]]



## Misc

### Cross Compilers

ARMEABIToolchain for Ubuntu
https://wiki.ubuntu.com/Toolchain/Crosscompilers/ARMEABIToolchain


GNU Tools for ARM Embedded Processors
https://launchpad.net/gcc-arm-embedded


### Coffeescript

Debug CoffeeScript
https://gist.github.com/AnsonT/1115264
http://stackoverflow.com/questions/11068023/debugging-coffeescript-line-by-line 


## Best Practices and Principles

- [Best Practices for Designing a Pragmatic RESTful API](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)

- [LiveScript Style Guide](https://github.com/gkz/LiveScript-style-guide)

### Immutable Infrastructure

- [Immutable Infrastructure Realized: Fugue Computing](http://luminal.com/blog/2014-11-06-fugue-computing.html)
- [Trash Your Servers and Burn Your Code: Immutable Infrastructure and Disposable Components](http://chadfowler.com/blog/2013/06/23/immutable-deployments/)
- [Virtual Panel on Immutable Infrastructure](http://www.infoq.com/articles/virtual-panel-immutable-infrastructure)
- [After Docker:
Unikernels and Immutable Infrastructure](https://medium.com/@darrenrush/after-docker-unikernels-and-immutable-infrastructure-93d5a91c849e)


### [The Twelve Factor App](http://12factor.net/)

The twelve-factor app is a methodology for building `software-as-a-service apps` that:

- Use **declarative** formats for setup automation, to minimize time and cost for new developers joining the project;
- Have a **clean contract** with the underlying operating system, offering **maximum portability** between execution environments;
- Are suitable for **deployment** on modern **cloud platforms**, obviating the need for servers and systems administration;
**Minimize divergence** between development and production, enabling **continuous deployment** for maximum agility;
- And can **scale up** without significant changes to tooling, architecture, or development practices.

`The Twelve Factors`

  - Codebase (One codebase tracked in revision control, many deploys)
  - Dependencies (Explicitly declare and isolate dependencies)
  - Config (Store config in the environment)
  - Backing Services (Treat backing services as attached resources)
  - Build, release, run (Strictly separate build and run stages)
  - Processes (Execute the app as one or more stateless processes)
  - Port binding (Export services via port binding)
  - Concurrency (Scale out via the process model)
  - Disposability (Maximize robustness with fast startup and graceful shutdown)
  - Dev/prod parity (Keep development, staging, and production as similar as possible)
  - Logs (Treat logs as event streams)
  - Admin processes (Run admin/management tasks as one-off processes)

