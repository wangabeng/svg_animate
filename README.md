# svg_animatesvg动画相关

# svg样式表
1 内联样式表  
```
<svg xmlns="http://www.w3.org/2000/svg">
     
    <style type="text/css" >
      <![CDATA[
 
        circle {
           stroke: #006600;
           fill:   #00cc00;
        }
 
      ]]>
    </style>
     
    <circle  cx="40" cy="40" r="24"/>
</svg>  
```
这种使用内联样式表的工作方式和在HTML元素上使用内联样式表是完全相同的。内联样式表可以在IE7和Firefox 3.0.5浏览器上正常工作。  

2 class样式  
你还可以为每个图形都添加一个class类，使用这个class类来在样式表中作为选择器选择相应的图形。  
```
<svg xmlns="http://www.w3.org/2000/svg">
 
    <style type="text/css" >
      <![CDATA[
 
        circle.myGreen {
           stroke: #006600;
           fill:   #00cc00;
        }
       circle.myRed {
       stroke: #660000;
       fill:   #cc0000;
    }
 
      ]]>
    </style>
 
    <circle  class="myGreen" cx="40" cy="40"  r="24"/>
    <circle  class="myRed"   cx="40" cy="100" r="24"/>
</svg> 
```

3 使用外部样式表  
```
<?xml-stylesheet type="text/css" href="svg-stylesheet.css" ?>
<svg xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink">
 
 
    <circle cx="40" cy="40" r="24"
       style="stroke:#006600; fill:#00cc00"/>
 
</svg>
```

4 使用属性来添加CSS样式(推荐)  
<circle stroke="#000000" fill="#00ff00" />  

5 使用style属性（不推荐）  
<circle style="stroke: #000000; fill:#00ff00;" />  

## svg基本操作API
```
1 document.getElementById根据元素ID来获取这个容器对象
如已经存在一个svg元素
<svg xmlns="http://www.w3.org/2000/svg" id="main" version="1.1" height="200"></svg>
可以这样获取这个元素：
var main = document.getElementById( "main" );

2 创建图形 
document.createElementNS(ns, tagName)
例如创建一个矩形： document.createElementNS( "http://www.w3.org/2000/svg", "rect" )

3 添加图形到svg容器内
element.appendChild(childElement)
例如：
main.appendChild( rect );

4 设置/获取属性
element.setAttribute(name, value)
element.getAttribute(name)
例如：
rect.setAttribute( "width", 100 );
rect.setAttribute( "height", 30 );
rect.setAttribute( "style", "fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0)" );

案例：往一个svg容器内添加一个矩形
<script>
    var main = document.getElementById( "main" );
    var rect = document.createElementNS( "http://www.w3.org/2000/svg", "rect" );
    rect.setAttribute( "width", 100 );
    rect.setAttribute( "height", 30 );
    rect.setAttribute( "style", "fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0)" );
    main.appendChild( rect );
</script>

5 设置文本
textContent属性可以获取和设置text元素文本
例如：
// SVG
<text id="text" x="25" y="25" font-size="16" style="fill:rgb(0,0,0);">foo</text>;
 
// JS
<script>
    var text = document.getElementById( "text" );
    console.log( text.textContent ); // foo
    text.textContent = "Hello world!"; // 重新设置文本
</script>

6 获取元素高宽和坐标
getBBox方法可以获取所有元素的高宽和坐标：
// SVG
<text id="text" x="25" y="25" font-size="16" style="fill:rgb(0,0,0);">foo</text>;
 
// JS
<script>
    var text = document.getElementById( "text" );
    console.log( text.getBBox() ); // {height: 16, width: 32, y: 11, x: 25} 
</script>

***重要***
7 事件处理
SVG也可以像HTML那样为元素添加自定义事件。
使用on + 事件名属性监听：
// SVG
<text id="text" x="25" y="25" font-size="16" style="fill:rgb(0,0,0);">foo</text>;
 
// JS
<script>
    var text = document.getElementById( "text" );
    // 点击文本时弹出其内容
    text.onclick = function() {
        alert( this.textContent );
    };
</script>

也可以使用element.addEventListener方法监听：
// SVG
<text id="text" x="25" y="25" font-size="16" style="fill:rgb(0,0,0);">foo</text>;
 
// JS
<script>
    var text = document.getElementById( "text" );
    // 点击文本时弹出其内容
    // 事件名前面不带on
    text.addEventListener( 'click', function() {
        alert( this.textContent );
    } );
</script>
两种方法有什么不同？on + 事件名属性只能为同一个事件添加一个处理方法，再对这个属性进行设置时会覆盖掉上一个方法，而element.addEventListener多次使用也不会覆盖上一个，而是从原来的事件上叠加。

8 更改svg内元素的z-index属性 貌似用不上 略 详见
https://blog.csdn.net/happyduoduo1/article/details/51789552

9 获取path的长度 pathele.getTotalLength()
例如
  var path = document.querySelector('path');
  var length = path.getTotalLength();
```
## svg坐标系
1 用户坐标系 也称原始坐标系 是svg元素创建的时候自带的坐标系  
2 自身坐标系 比如绘制了一个矩形 那么这个矩形自身就带一个坐标系 就叫做自身坐标系 矩形定义的x y就是基于用户坐标系 而 width height就是基于自身坐标系的。  
3 前驱坐标系 就是父容器的坐标系，比如这个矩形 父容器就是svg。这个矩形如果添加transform='translate(50,50)',那么这个矩形就相对于前驱坐标系做了一个变换（矩形整体下移50 右移50），但是其自身坐标系没有变。  
4 参考坐标系，比如矩形的参考坐标系就是父元素的坐标系。

## 力导向图
力导向算法原理分析 见 http://blog.sina.com.cn/s/blog_6a1bf1310101hc7l.html#  
基本点：  
1 库仑力是静止带电体之间的相互作用力。计算公司 F=K(Q1Q2)/r²（r是两个点电荷之间的距离，Ke是库仑常数 等于9*10^9N/C）

-- 待续  

## SVG多分辨率、自适应缩放解决方案
https://www.w3cplus.com/html5/svg-coordinate-systems.html
1 在SVG根标签中添加 preserveAspectRatio="xMinYMin meet" 属性，该属性指定SVG图形在屏幕的最左上角开始显示，并且保持等比缩放。  

2 在SVG跟标签中添加 viewBox 属性  并设置width height

完整实例代码:
```
<svg width='3rem' height='3rem'  preserveAspectRatio="xMinYMin meet" xmlns="http://www.w3.org/2000/svg"   xmlns:xlink="http://www.w3.org/1999/xlink">
    <circle cx="300" cy="300" r="200" stroke="gray" stroke-width="10"
fill="transparent"/>
</svg>
```

## 元素分组
1 <g> <use>配合
```
<svg  width='900' height='900' xmlns="http://www.w3.org/2000/svg">
 <g id='testgroup'>
   <line x1='300' y1='300' x2='400' y2='400'  stroke='red' stroke-width='3' />
 </g>
 <use xlink:href='#testgroup'  transform="rotate(5)" />
</svg>
```
缺点 无法自定义use的样式及内容

2 解决1的问题 用defs
```
<svg viewBox = "0 0 1000 1000" version = "1.1">
    <defs>
        <circle id = "s1" cx = "200" cy = "200" r = "200" fill = "yellow" stroke = "black" stroke-width = "3"/>
        <ellipse id = "s2" cx = "200" cy = "150" rx = "200" ry = "150" fill = "salmon" stroke = "black" stroke-width = "3"/>
    </defs>
    <use x = "100" y = "100" xlink:href = "#s1"/>
    <use x = "100" y = "650" xlink:href = "#s2"/>
</svg>
```
# 坐标变换，注意先平移再放大 和先放大再平移的区别
件svg精髓63-64页介绍

# svg围绕中心旋转 加2个参数 及中心点的坐标即可
transform='rotate(角度,centerX值,centerY)'

# 普通html使用svg的filter滤镜 就像给html添加了ps 案例
```
<!DOCTYPE HTML>
<html>
<meta charset="utf-8">
<head>
<title>Analog Clock</title>
<style>
p.target { filter:url(#f3); }
p.target:hover { filter:url(#f5); }
b.target { filter:url(#f1); }
b.target:hover { filter:url(#f4); }
pre.target { filter:url(#f2); }
pre.target:hover { filter:url(#f3); }

</style>
</head>
<body>

<p class="target" style="background: lime;">
    Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt
    ut labore et dolore magna aliqua. Ut enim ad minim veniam.
</p>
<pre class="target">lorem</pre>
<p>
    Lorem ipsum dolor sit amet, consectetur adipisicing
    <b class="target">elit, sed do eiusmod tempor incididunt
    ut labore et dolore magna aliqua.</b>
    Ut enim ad minim veniam.
</p>
<svg height="0">
  <filter id="f1">
    <feGaussianBlur stdDeviation="3"/>
  </filter>
</svg><svg height="0">
  <filter id="f2">
    <feColorMatrix values="0.3333 0.3333 0.3333 0 0
                           0.3333 0.3333 0.3333 0 0
                           0.3333 0.3333 0.3333 0 0
                           0      0      0      1 0"/>
  </filter>
</svg>
<svg height="0">
  <filter id="f3">
    <feConvolveMatrix filterRes="100 100" style="color-interpolation-filters:sRGB"
      order="3" kernelMatrix="0 -1 0   -1 4 -1   0 -1 0" preserveAlpha="true"/>
  </filter>
  <filter id="f4">
    <feSpecularLighting surfaceScale="5" specularConstant="1"
                        specularExponent="10" lighting-color="white">
      <fePointLight x="-5000" y="-10000" z="20000"/>
    </feSpecularLighting>
  </filter>
  <filter id="f5">
    <feColorMatrix values="1 0 0 0 0
                           0 1 0 0 0
                           0 0 1 0 0
                           0 1 0 0 0" style="color-interpolation-filters:sRGB"/>
  </filter>
</svg>

</body>
</html>
```

# SVG中stroke-dasharray及stroke-dashoffset属性 
stroke-dasharray属性用来设置描边的点划线的图案范式。就是设置实线和虚线的宽度  

比如：  
stroke-dasharray: 50 20;   
效果就是：  
![image](/a.png)
50和20分别对应了实线和虚线的长度

stroke-dashoffset则指定了dash模式到路径开始的距离，就是实线虚线绘制的起点距路径开始的距离  

比如：   
stroke-dashoffset: 30;   
效果： 
![image](/b.png)
由于向左移动30像素，这样左边的实线就跟虚线一样长了。

我们可以设置stroke-dashoffset与stroke-dasharray相同的值实现“画线”的效果： 
代码：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no"/>
    <meta content="yes" name="apple-mobile-web-app-capable">
    <meta content="black" name="apple-mobile-web-app-status-bar-style">
    <meta content="telephone=no" name="format-detection">
    <meta content="email=no" name="format-detection">

    <title>XXX-xxx</title>
    <link href="./css/awesome.css" rel="stylesheet">
    <link href="./css/base.css" rel="stylesheet">
    <link href="./css/common.css" rel="stylesheet">
    <link href="./css/iSlider.css" rel="stylesheet">
    <link href="./css/index.css" rel="stylesheet">
    
    <script type="text/javascript" src="./js/jquery1.10.2.js"></script>
    <script type="text/javascript" src="./js/fontSize.js"></script>
    <script type="text/javascript" src="./js/common.js"></script>
    <script type="text/javascript" src="./js/iSlider.js"></script>
    <script type="text/javascript" src="./js/iSlider.animate.js"></script>
    <script type="text/javascript" src="./js/iSlider.dot.js"></script>
    <script type="text/javascript" src="./js/iSlider.plugin.button.js"></script>
    <script type="text/javascript" src='./layer/layer.js'></script>
    <style>
        /*可以设置svg视野大小 可理解为工作区大小 然后svg内path的尺寸会跟着缩放*/
        path.graphic {
          /*stroke-dasharray: 2204.536865234375;*/
          /*stroke-dashoffset: 2204.536865234375;*/
          animation: dash 2s linear forwards;
        }

        @keyframes dash {
          to {
            stroke-dashoffset: 0;
          }
        }

    </style>
</head>
<body>
<div>
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
   width="940.5px" height="844.5px" viewBox="0 0 940.5 844.5" enable-background="new 0 0 940.5 844.5" xml:space="preserve">
    <g>
      <path class='graphic' fill="none" stroke="#000000" stroke-width="20" d="M264.477,268.949c26.679,57.321,146.993-166.94,241.488-123.325
        c94.495,43.615,218.987,7.52,290.985,209.051c71.995,201.53,17.315,332.376-190.082,333.88
        c-207.398,1.504-402.39,69.183-421.889-206.043C165.48,207.285,208.979,71.928,225.478,171.19
        C241.977,270.453,253.977,246.389,264.477,268.949z"/>
    </g>
</svg>


<img src="./img/svg.svg" alt="">
</div>
</body>
<script>
    $(function () {
        // 计算path图形总长度 然后设置其 stroke-dasharray stroke-dashoffset为这个取得的值
        !function () {
            var pathEle = document.querySelector('.graphic');
            var length = pathEle.getTotalLength();
            pathEle.setAttribute('stroke-dasharray', length);
            pathEle.setAttribute('stroke-dashoffset', length);
            // console.log(length);
        }();

    });
</script>
</html>
```

# SVG动画的三种方式
1 css3 设置动画帧 frame 不同的帧 修改不同的svg元素的属性值（如画一个logo案例）  
2 利用js 设置不同时间段（setAttribute不同的帧的transform值）（见svg钟案例）  
3 使用animatetranform 例如：
```
<svg xmlns="http://www.w3.org/2000/svg" width="300px" height="200px">
    <rect x="0" y="0" width="200" height="200" fill="yellow" stroke="red" stroke-width="1" />
    <rect x="20" y="50" width="15" height="34" fill="blue" stroke="black" stroke-width="1" transform="rotation">
        <!-- Rotate from 0 to 360 degrees, and move from 60 to 100 in the x direction. -->
        <!-- Keep doing this until the drawing no longer exists. -->
        <animateTransform attributeType="XML" attributeName="transform" begin="0s" dur="5s" type="rotate" from="20 60 60" to="360 100 60" repeatCount="indefinite" />
    </rect>
</svg>
```
4 设置轨迹动画 用animateMotion 详见《svg精髓》。
