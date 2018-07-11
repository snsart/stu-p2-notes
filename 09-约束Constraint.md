通过Constraint相关类，为两个刚体之间制定了互相制约的关系。
### Constraint
所有约束的基类
###### 构造函数
Constraint ( bodyA,bodyB,type,[options] )<br>
有一个可选参数：[collideConnected=true]

### RevoluteConstraint
旋转约束，两个刚体绕着一个枢轴旋转；

##### RevoluteConstraint ( bodyA,bodyB,[options] )

bodyA Body<br>
bodyB Body<br>

[worldPivot] Array optional<br>
全局枢轴，如果提供了这个参数，localPivotA和localPivotB会根据这个参数自动计算出来；<br>

[localPivotA] Array optional<br>
bodyA的质心所围绕的枢轴<br>

[localPivotB] Array optional<br>
bodyB的质心围绕的枢轴<br>

[maxForce] Number optional<br>
能加在这个系统中的最大的力<br>

下面这个例子通过两种方式（分别用全局和本地枢轴）创建了两个互相环绕刚体：
```javaScript
// This will create a revolute constraint between two bodies with pivot point in between them.
  var bodyA = new Body({ mass: 1, position: [-1, 0] });
  var bodyB = new Body({ mass: 1, position: [1, 0] });
  var constraint = new RevoluteConstraint(bodyA, bodyB, {
      worldPivot: [0, 0]
  });
  world.addConstraint(constraint);
        
        // Using body-local pivot points, the constraint could have been constructed like this:
  var constraint = new RevoluteConstraint(bodyA, bodyB, {
      localPivotA: [1, 0],
      localPivotB: [-1, 0]
  });
```
