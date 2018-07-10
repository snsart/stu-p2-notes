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

### 属性
##### velocity Array
刚体的速度，velocity[0]为x轴上的速度，velocity[1]为y轴上的速度；

### 示例：创建一个刚体
```typeScript
characterBody = new p2.Body({
    mass: 1,
    position:[0,3],
    fixedRotation: true,
    damping: 0.5
});
```
