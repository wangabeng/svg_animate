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
