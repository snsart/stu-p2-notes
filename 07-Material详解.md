# Material
通过Material(材质)可以设置两个shape之间的摩擦力等属性，在使用时需要借助ContactMaterial类；<br>
Material只有一个属性：id<br>

### ContactMaterial
定义了两种材料接触时会发生什么，比如可以规定材料之间的摩擦系数。你也可以做一下其他设置，比如恢复，表明速度和约束参数;<br>
##### 参数
ContactMaterial ( materialA  materialB  [options] )
```typeScript
materialA Material
materialB Material
[options] Object optional
[friction=0.3] Number optional
Friction coefficient.

[restitution=0] Number optional
Restitution coefficient aka "bounciness".

[stiffness] Number optional
ContactEquation stiffness.

[relaxation] Number optional
ContactEquation relaxation.

[frictionStiffness] Number optional
FrictionEquation stiffness.

[frictionRelaxation] Number optional
FrictionEquation relaxation.

[surfaceVelocity=0] Number optional
Surface velocity.
```
### 用法
下面定义了三种材质，并设置了材质之间互相接触时的摩擦系数；
```typeScript
var groundMaterial = new p2.Material(),
    characterMaterial = new p2.Material(),
    boxMaterial = new p2.Material();
  
var groundCharacterCM = new p2.ContactMaterial(groundMaterial, characterMaterial,{
     friction : 0, // No friction between character and ground
});
var boxCharacterCM = new p2.ContactMaterial(boxMaterial, characterMaterial,{
     friction : 0, // No friction between character and boxes
});
var boxGroundCM = new p2.ContactMaterial(boxMaterial, groundMaterial,{
     friction : 0.6, // Between boxes and ground
});

//把材质之间的联系类加入world
world.addContactMaterial(groundCharacterCM);
world.addContactMaterial(boxCharacterCM);
world.addContactMaterial(boxGroundCM);
```
然后通过shape的material属性，把材质赋予shape即可：
```typeScript
var boxShape = new p2.Box({
    width: 0.8,
    height: 0.8,
    material: boxMaterial
});
```


