
### [Server-Side Device Detection With JavaScript](http://www.smashingmagazine.com/2014/07/01/server-side-device-detection-with-javascript/)

Embedded Code:

```html
<script type='text/javascript' 
        src=“//wurfl.io/wurfl.js"></script>
```

Results:

```javascript
{
  complete_device_name:"Apple iPhone 5",
  form_factor:"Smartphone",
  is_mobile:true
}
```

### Articles

- [Enhancing User Experience With The Web Speech API](http://www.smashingmagazine.com/2014/12/05/enhancing-ux-with-the-web-speech-api/)

- [JavaScript Application Architecture On The Road To 2015](https://medium.com/@addyosmani/javascript-application-architecture-on-the-road-to-2015-d8125811101b)

- [Web 前端开发精华文章推荐（jQuery、HTML5、CSS3）](http://www.cnblogs.com/lhb25/p/must-read-links-for-web-designers-and-developers-volume-12.html)

### Resources

#### [Framer](http://framejs.com), Prototype Interaction and Animation

Framer.js is an open source JavaScript framework for rapid prototyping. Framer.js allows you to define animations and interactions, complete with filters, spring physics, 3D effects and more. It's bundled with Framer Generator, an application that allows you to import layers directly out of Photoshop and Sketch.

[用Framer.js构建可交互、易实现原型](http://www.ui.cn/project.php?id=21472)

#### [duo](https://github.com/duojs/duo), A next-generation package manager for the front-end
Duo is a next-generation package manager that blends the best ideas from Component, Browserify and Go to make organizing and writing front-end code quick and painless.


### Chart Libraries

#### [ChartJS](http://www.chartjs.org/)

Simple, clean and engaging charts for designers and developers
![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/11/1414814931chartjs.jpg)

- independent library (no dependencies to 3rd-party)
- html5 canvas
- charts: line, bar, radar, polar, and pie/doughnut charts

#### [Flot](http://www.flotcharts.org/)

a pure JavaScript plotting library for jQuery, with a focus on simple usage, attractive looks and interactive features.
![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/11/1414814939flot.jpg)

#### [MeteorCharts](https://github.com/ericdrowell/MeteorCharts)

MeteorCharts is the next generation charting framework for the web. It uses an abstract layer for data binding, and therefore supports any rendering technology of your choice, including HTML5 Canvas, WebGL, SVG, VML, and Dom.

- target: html5 canvas, webgl, svg, vml, and dom
- nodejs backend library

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/11/1414814945meteorcharts.jpg)


#### [ECharts](http://echarts.baidu.com/doc/example.html)

Developed by Baidu, and cool!!
![](http://images.cnitblog.com/blog/36987/201412/112222139008732.png)


### Obfuscation

[12 Days of HaXmas: Improvements to jsobfu](https://community.rapid7.com/community/metasploit/blog/2014/12/27/improvements-to-jsobfu)

```text
$ echo "console.log('Hello World')" | jsobfu

window[(function () { var E="ole",d="ons",f="c"; return f+d+E })()][(String.fromChar
Code(108,111,0147))](String.fromCharCode(0x48,0x65,0154,0154,111,32,0127,0x6f,114,01
54,0x64));
```

There is also an optional `iterations` parameter that allows you to obfuscate a specified number of times:

```text
$ echo "console.log('Hello World')" | jsobfu 3

window[(function(){var T=String[(String.fromCharCode(102,114,0x6f,109,0x43,104,97,0x
72,0x43,0157,0x64,0145))](('j'.length*0x39+54),('h'.length*(3*('X'.length*024+8)+9)+
15),(1*('Q'.length*(1*0x40+14)+19)+4)),Z=(function(){var c=String.fromCharCode(0x6e,
0163),I=String.fromCharCode(99,0x6f);return I+c;})();return Z+T;})()][(String[(Strin
g[((function () { var r="de",t="mCharCo",M="f",_="ro"; return M+_+t+r })())]((0x6*0x
f+12),(01*('J'.length*('z'.length*(4*0x9+4)+27)+1)+46),(0x37*'Bw'.length+1),('K'.len
gth*(0x3*0x1a+17)+14),(02*(1*(1*(05*'RIZ'.length+2)+6)+3)+15),('X'.length*('zzJA'.le
ngth*021+15)+21),(0x1*0111+24),('FK'.length*0x2b+28),('z'.length*0x43+0),(03*33+12),
('AZa'.length*('NKY'.length*(02*4+3)+0)+1),(1*0x5c+9)))](('u'.length*(01*('KR'.lengt
h*('av'.length*0x7+3)+5)+19)+(01*('j'.length*056+0)+4)),('z'.length*(String.fromChar
Code(0x67,85,0155,0156,75,84,0114,0x4c)[((function () { var f="ngth",F="e",x="l"; re
turn x+F+f })())]*((function () { var n='m',a='Q'; return a+n })()[(String.fromCharC
ode(0154,101,110,0x67,0x74,104))]*(function () { var w='d',A='tMf'; return A+w })()[
((function () { var yG="ngth",q5="e",J="l"; return J+q5+yG })())]+'SX'.length)+'crFi
Kaq'.length)+(1*026+2)),('p'.length*(06*15+10)+'nnU'.length)))]((function(){var En=S
tring[(String.fromCharCode(0146,0x72,0x6f,0x6d,0103,104,97,0x72,67,0x6f,0144,101))](
(3*041+9),('eHUOhZL'.length*(0x1*(01*9+1)+3)+9)),Y=(function(){var z=(function () {
var Sf='r'; return Sf })(),Z=(function () { var N='o'; return N })(),C=String.fromCh
arCode(0x57);return C+Z+z;})(),k=String[((function () { var b="e",s="od",p="fromCha"
,H="rC"; return p+H+s+b })())](('C'.length*('H'.length*('Ia'.length*0xf+3)+12)+27),(
'G'.length*(01*('Wv'.length*25+10)+27)+14),('Q'.length*077+45),('MXq'.length*30+18),
(1*('B'.length*(0x1*29+20)+24)+38),(0x2*020+0));return k+Y+En;})());
```

### Waiting Screen

[please-wait](http://pathgather.github.io/please-wait/)