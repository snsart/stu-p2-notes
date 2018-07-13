# world详解

### 事件
* world继承自EventEmitter，因此拥有下列方法：<br>
on(type,listener)注册事件侦听器<br>
off(type,listener)移除事件侦听器<br>
has(type,listener)检查一个事件侦听器是否被添加<br>
emit(event)发送事件<br>

* p2中定义的事件类型有：<br>
##### addBody:
当有刚体加入world中时触发;<br>
##### removeBody:
当有刚体从world中移除时触发；<br>
下面示例中e.body会获得刚加入的刚体；
```typeScript
world.on("addBody",function(e){
  var body=e.body;
})
```
##### beginContact:
当world中有两个shape发生重叠或两个刚体发生碰撞时触发,Event Payload:shapeA,shapeB,bodyA,bodyB分别代表发生碰撞的shape或body
##### endContact：
当world中有两个shape或body分离时触发，Event Payload:shapeA,shapeB,bodyA,bodyB分别代表分离时的shape或body。<br>

##### preSolve：
碰撞求解前触发，求解就是指根据碰撞时用到的约束方程式计算碰撞后的效果，比如冲击力，弹力等，需要改变碰撞效果或需要计算碰撞冲击力造成的破坏等效果时，需要使用此事件。可以在事件的回调函数中通过改变约束方程式中的属性来改变碰撞效果；如下例，在触发求解事件时，通过设置约束方程式的enabled值，使 characterBody和passThroughBody之间的接触约束和摩擦约束均不起作用：
```typeScript
world.on('preSolve', function (evt){
  for(var i=0; i<evt.contactEquations.length; i++){
      var eq = evt.contactEquations[i];
      if((eq.bodyA === characterBody && eq.bodyB === passThroughBody) || eq.bodyB === characterBody && eq.bodyA === passThroughBody){
          eq.enabled = false;//使接触约束不起作用
      }
  }
  for(var i=0; i<evt.frictionEquations.length; i++){
      var eq = evt.frictionEquations[i];
      if((eq.bodyA === characterBody && eq.bodyB === passThroughBody) || eq.bodyB === characterBody && eq.bodyA === passThroughBody){
          eq.enabled = false;//使摩擦约束不起作用
      }
  }
});
```
##### postStep：
碰撞求解后触发，碰撞求解后的回调函数，需要计算碰撞冲击力造成的破坏等效果时，需要使用此事件。

另外还有：impact、postBroadphase等事件类型，具体参考[world API文档](http://schteppe.github.io/p2.js/docs/classes/World.html)<br>

总而言之，加入world中的所有形状和刚体,当他们发生改变时（碰撞、移除、发生联系）,要通过在world上注册事件来捕获它们的行为，对它们进行管理；

### 方法
##### 添加和移除
在world中可以添加和移除body、Constraint(约束)、ContactMaterial(接触材料)、Sring，具体有：<br>
addBody<br>
addConstraint<br>
addContactMaterial<br>
addSpring<br>
removeBody<br>
removeConstraint<br>
removeContactMaterial<br>
removeSpring<br>
##### clear方法
会重置世界,移除所有的bodies,constraints和springs.
##### get set相关方法
getBodyById<br>
getContactMaterial<br>
setGlobalRelaxation<br>
setGlobalStiffness<br>
```typeSctipt
world.setGlobalStiffness(1e5);//设置world中各物体的硬度
```

##### 取消和添加两个刚体之间的碰撞行为
disableBodyCollision ( bodyA,bodyB )<br>
enableBodyCollision ( bodyA,bodyB )<br>

##### hitTest
hitTest(worldPoint,bodies,precision):Array<br>
检测bodies数组中的刚体是否和某点重叠<br>
最后一个参数是精度，多用在有粒子和线条的系统中<br>
返回所有与worldPoint重叠的刚体；

##### step
step ( dt , [timeSinceLastCalled=0] ,[maxSubSteps=10])<br>
step方法用来推进世界各物体的运行<br>
dt-物理引擎中所用的最小时间间隔单位，一般设置为fpt的倒数，常为1/60,每当p2.js让时间前进的时候，它都会推进它内部的物理时钟用这个参数值。<br>
timeSinceLastCalled-可选参数,规定每隔多长的时间唤醒 world.step()方法。p2.js有个内置的“挂钟”，会把每隔多长这个时间的累积量映射出来。每执行一次step，p2中的数据会更新一次，因此这个值越小，渲染出的物理世界推进的越慢；<br>
maxSubSteps-当你把三个参数都传给world.step()方法时，p2.js会执行fixed steps直到“物理时钟”和“挂钟”的时间同步了（dt*fixed steps=timeSinceLastCalled）。<br>
这个花招是为了得到独立的帧速率。最后一个参数 maxSubSteps就不要解释了：这个就是每次使用world.step()方法时，规定最大的fixed steps。切记，timeSinceLastCall 的值总是要小于 maxSubSteps * fixedTimeStep，否则的话你就是在流失时间了。<br>
减少fixedTimeStep（固定时间步），可以增加模拟的世界的“分辨率”。如果你发现你的物体移动得非常快，并且不经碰撞直接脱离墙体，那么你可以通过减少时间步fixedTimeStep来纠正这个问题。并且要记得增加maxSubSteps的值以确保符合要求timeSinceLastCall < maxSubSteps * fixedTimeStep。

另外还有：raycast、runNarrowphase方法，文档上讲的很细，这里不再记录。

### 属性
##### bodies Array
存储world中的所有刚体

##### defaultContactMaterial
设置world中各刚体的默认接触材质，可以通过这个属性设置摩擦力等属性<br>
```typeSctipt
world.defaultContactMaterial.friction = 0.5//将摩擦力设置为0.5；
```
##### world.narrowphase 
这个属性使用在所有产生接触的地方;<br>
world.narrowphase.contactEquations  取得所有接触约束方程;<br>
world.narrowphase.frictionEquations  取得所有摩擦约束方程；<br>
