# body详解
这里是：[bodyAPI文档](http://schteppe.github.io/p2.js/docs/classes/Body.html)<br>

### 结构体
Constructor中有十几个可选参数，常用的有：<br>
position：Array-设置刚体位置positon[0]为x轴，position[1]为y轴；
angle:Number-设置刚体角度；
mass:Number-设置刚体质量；
id:Number-设置刚体id值

### 事件

body继承自EventEmitter类，因此拥有on() off() has() emit事件相关方法。拥有的事件类型有:<br>
sleep：刚体休眠时触发；<br>
sleepy：暂时不理解；<br>
wakeup：刚体活动时触发；<br>

### Methods
* applyImpulse ( impulse  [relativePoint] )<br>
给刚体添加一个推力，推力会改变刚体的速度和角速度；<br>
impulse Array<br>
推力的向量<br>
[relativePoint] Array optional<br>
可选参数，指推力的着力点相对刚体质心的偏移量，默认为[0,0],即刚体的质心。<br>

### 属性
* velocity Array<br>
刚体的速度，velocity[0]为x轴上的速度，velocity[1]为y轴上的速度；<br>

* interpolatedPosition<br>
插值位置，此属性会填充两个position之间的空隙，在渲染时要用此属性获取刚体的位置来得到连续的动画，不要使用position；<br>

* interpolatedAngle<br>
插值角度，和插值位置一样，在渲染时也要使用此属性来获取刚体的旋转角度；<br>

* type <br>
刚体的类型有三种：<br>
**Body.STATIC**——刚体不可以移动，但是可以与 dynamic类型刚体交互。
**Body.DYNAMIC**——刚体可以与任何类型的刚体交互，可以移动。
**Body.KINEMATIC**——刚体通过设置速度来控制，其他方面则和Static刚体相同。

### 示例：创建一个刚体
```typeScript
characterBody = new p2.Body({
    mass: 1,
    position:[0,3],
    fixedRotation: true,
    damping: 0.5
});
```
