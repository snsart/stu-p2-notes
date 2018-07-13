通过Constraint相关类，为两个刚体之间制定了互相制约的关系。constraint 是一个物理连接件，用来控制刚体的自由度。在3d世界，物体有6个自由度（3个平移坐标和3个旋转坐标）。在2d世界，物体只有3个自由度（2个平移坐标和1个旋转坐标）。众所周知，人类世界是3d的，因此我们家里的门本来应该是有6个自由度的，但是由于门的一侧被门铰链固定在墙上，它失去了另外5个自由度，只能照着门铰链这个轴旋转了。门铰链就相当于一个constraint（约束）。
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
### Contact constraint（接触约束）
这是一个特别的约束，作用在于防止刚体之间的渗透重叠，并且它可以模拟摩擦和弹性。你无须创建这个约束，系统会自动创建它的。
