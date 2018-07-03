### 获得p2资源库
[这里是p2引擎的github链接](https://github.com/egret-labs/egret-game-library/tree/master/physics)
### 在egret中配置p2
这里是在egret中构建第三方库的方法：[如何构建第三方库](http://developer.egret.com/cn/github/egret-docs/Engine2D/projectConfig/libraryProject/index.html)<br>
不过上面链接中的资源是已经构建好的，我们只需要在项目中的项目配置文件(egretProperties.json)中引入外部模块：<br>
```json
{
  "engineVersion": "5.2.4",
  "compilerVersion": "5.2.4",
  "template": {},
  "target": {
    "current": "web"
  },
  "modules": [
    {
      "name": "egret"
    },
    {
      "name": "assetsmanager"
    },
    {
      "name": "game"
    },
    {
      "name": "tween"
    },
    {
      "name": "physics",//p2引擎的名字
      "path": "../libsrc"//资源的相对路径
    },
    {
      "name": "promise"
    }
  ]
}
```
然后编译项目即可
