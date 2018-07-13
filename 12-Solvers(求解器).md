# Solvers
一个solver就是一条解决一个线性方程组的运算法则。在p2.js,它处理着约束，接触，摩擦。
### GSSolver（高斯求解器）
在p2.js中有两种求解器，高斯求解器GSSolver是最稳固的了。这个求解器是重复的，设定好重复次数iterations后它能得出一个较平均的解。通常，重复次数越多，解也就越精确。
```javaScript
world.solver = new GSSolver();
world.solver.iterations =  5; // Fast, but contacts might look squishy...
world.solver.iterations = 50; // Slow, but contacts look good!
```
### Island solving（岛屿处理）
不同于一次性处理整个系统，Island solving（岛屿处理）会把整体分割成独立的部分（亦称“岛屿”），然后去分别处理它们。<br>
岛屿并行处理，效果显然是最好的。而且有助于在单线程的状态下连续处理，特别是当solver tolerance求解器的容差大于0的时候。求解器能够很早的就保释一些岛屿，其他的岛屿则得到更多的重复次数。
```javaScript
var world = new p2.World({
    islandSplit: true
});
world.solver.tolerance = 0.01;
```

### Solver parameters（求解器参数）
求解器的参数在Equation对象上设置。你要提供硬度和放松度，像这样：
```javaScript
equation.stiffness = 1e8;
equation.relaxation = 4;
equation.updateSpookParams(timeStep);
```
可以把硬度stiffness想象成一个弹簧spring的硬度，这个弹簧给出一个F=-k*x 的力，X代表弹簧的位移。放松度relaxation和时间步的数量一致，我们用这个来稳定约束（Relaxation 值越大，接触越柔软）。<br>
ContactEquation（接触方程） 和 FrictionEquation（摩擦方程）是最主要的方程类。这些方程是自动创建的。想改变接触的硬度和放松度，设置ContactMaterial的属性值：
```javaScript
contactMaterial.stiffness = 1e8;
contactMaterial.relaxation = 3;
contactMaterial.frictionStiffness = 1e8;
contactMaterial.frictionRelaxation = 3;
```
你也可以设置硬度和放松度在Constraint equations。只需遍历它所有的的方程式即可。<br>


>参考文件：[csdn](https://blog.csdn.net/qilei2010/article/details/51901741#dynamics%E5%8A%9B%E5%AD%A6);

