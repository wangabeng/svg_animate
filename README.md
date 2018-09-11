# svg_animatesvg动画相关

## svg基本操作API
```
创建图形
document.createElementNS(ns, tagName)
添加图形
element.appendChild(childElement)
设置/获取属性
element.setAttribute(name, value)
element.getAttribute(name)
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
https://blog.csdn.net/kungstriving4/article/details/25186553  
1 在SVG根标签中添加 preserveAspectRatio="xMinYMin meet" 属性，该属性指定SVG图形在屏幕的最左上角开始显示，并且保持等比缩放。  
2 在SVG跟标签中添加 viewBox 属性  
该属性来设置SVG画布的大小，但该大小是一个相对的大小，并不是绝对尺寸大小。比如设定一个viewBox="0,0,800,3000"，可以认为将画布大小的宽分为800份，高分为3000份，然后所有SVG的元素都在800*3000所分割成的画布上摆放。这时候不用考虑屏幕实际高宽。再次提醒注意：viewBox将画布分为800*3000份的小格子，然后所有的元素在该画布上摆放。至于该格子的绝对大小，则根据我们后面所设定的width height单位来决定。
  
3 添加SVG元素。  
注意，各个SVG元素的高宽和xy坐标都按照800*3000的大小来设置，也就是说如果想要在最右下角放一个100*100的矩形，那么就应该写成
```
<rect x="700" y="2900" width="100" height="100" fill="red" />
```
 我们上面已经说过，这时候不用关心实际屏幕的高宽，只按照viewBox画布大小进行设置。此时的 width="100" height="100"不是绝对数值 而是

完整实例代码:
```
<svg width='3rem' height='3rem'  preserveAspectRatio="xMinYMin meet" xmlns="http://www.w3.org/2000/svg"   xmlns:xlink="http://www.w3.org/1999/xlink">
    <circle cx="300" cy="300" r="200" stroke="gray" stroke-width="10"
fill="transparent"/>
</svg>
```

