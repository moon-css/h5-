# cocos框架和matter.js融合
> 以下所有函数均在`game.js`中定义，在`TLayer.js`中调用

## 文件导入

```js
{
	"name": "matter",
	"url": "libs/matter.js",
	"sync": true
},
{
	"name": "decomp",
	"url": "libs/decomp.min.js",
	"sync": true
},
```
ps:注意这边一定要引入decomp文件，否则无法正确绘制凹多边形


## 一、函数定义
1. 引入
> 初始化matter，创建物理引擎世界
```js
initMatter:function(){
    var Engine = Matter.Engine;
    game.Render = Matter.Render;
    game.Composite = Matter.Composite;
    game.Runner = Matter.Runner;
    game.Bodies = Matter.Bodies;
    game.Vertices = Matter.Vertices;
    game.Vector = Matter.Vector;

    game.runner = game.Runner.create();
    game.engine = Engine.create();
    game.world = game.engine.world;
  
    game.render = game.Render.create({
        element: document.getElementById('world'),
        engine: game.engine
    });
},
```

2. 添加物体
> 创建物体，并且加入物体数组中，方便之后放入物理引擎世界
```js
addBody:function(style,options){
    var body;
    if(style == 'circle'){
        body = game.Bodies.circle(options.x,options.y,options.radius)
    }else if(style == 'polygon'){
        body = game.Bodies.polygon(options.x,options.y,options.sides,options.radius)
    }else if(style == 'rectangle'){
        body = game.Bodies.rectangle(options.x,options.y,options.width,options.height)
    }else if(style == 'trapezoid'){
        body = game.Bodies.trapezoid(options.x,options.y,options.width,options.height,options.slope)
    }else if(style == 'Vertices'){
        body = game.Bodies.fromVertices(options.x,options.y,options.vertexSets)
    }
    if(options.isStatic){
        body.isStatic = true;//判断物体是不是刚体
    }
    this.bodyList.push(body);
    return body;
},
```

3. 将物体添加进物理引擎世界并运行物理引擎

```js
addToWorld:function(callback){
    game.Composite.add(game.world, game.bodyList);
    callback && callback();
},
startMatter:function(){
    game.Runner.run(game.runner, game.engine);
}
```


## 二、函数调用



### 案例展示

```js
var name1 = this.getChildByName('name_1');
name1.visible = true;
var name2 = this.getChildByName('name_2');
name2.visible = true;
var wall = this.getChildByName('wall_1');

//地面
var ground = game['addBody']('rectangle',{
    x:375,
    y:1180,
    width:750,
    height:60,
    isStatic:true
})


//物体信息，正式项目时从信息文件中引用
var name1Vertices =[ [52,7], [58,5], [73,9], [76,21], [81,26], [81,37], [69,47], [74,58], [62,81], [44,81], [36,69], [27,74], [19,69], [18,64], [3,61], [0,58], [0,50], [7,41], [24,2], [26,0], [38,0] ];
var name2Vertices =[ [58,3], [70,6], [70,15], [79,20], [79,30], [69,41], [75,52], [66,76], [61,78], [50,78], [46,76], [37,69], [29,74], [22,70], [20,64], [0,63], [0,53], [6,45], [19,0], [34,0] ];
var wallVertices= [ [256,94], [301,111], [301,116], [288,149], [284,158], [279,158], [271,155], [121,95], [4,48], [0,46], [0,42], [16,2], [17,0], [21,0] ];


var name1_ver = [],name2_ver = [],wallVer = [],tigerVer = [],seats = '';


name1_ver = name1Vertices.map((p) =>game.Vector.create(p[0], p[1]));
name2_ver = name2Vertices.map((p) =>game.Vector.create(p[0], p[1]));
wallVer = wallVertices.map((p) =>game.Vector.create(p[0], p[1]));

//ps中刚体
var wall_1 = game['addBody']('Vertices',{
    x:wall.x,
    y:1240-wall.y,
    vertexSets:wallVer,
    isStatic:true
})

var boxA = game['addBody']('Vertices',{
    x:name1.x,
    y:1240-name1.y,
    vertexSets:name1_ver
})



var boxB = game['addBody']('Vertices',{
    x:name2.x,
    y:1240-name2.y,
    vertexSets:name2_ver
})
game['addToWorld'](function(){
    game['startMatter']();

})

//获取物体坐标的最大最小值
var name1_info = game.getCenter(name1Vertices);
var name2_info = game.getCenter(name2Vertices);
var wallInfo = game.getCenter(wallVertices);

var c1 = game['Vertices'].centre(name1_ver);
var c2 = game['Vertices'].centre(name2_ver);
var wall_c = game['Vertices'].centre(wallVer);

//将需要运动的物体的锚点设置在物体的重心上
name1_anchorX = c1.x/(name1_info.max_x-name1_info.min_x);
name1_anchorY = (name1_info.max_y-c1.y)/(name1_info.max_x-name1_info.min_x);//物理引擎中原点在左上，这里需要换成左下原点

name2_anchorX = c2.x/(name2_info.max_x-name2_info.min_x);
name2_anchorY = (name2_info.max_y-c2.y)/(name2_info.max_x-name2_info.min_x);//物理引擎中原点在左上，这里需要换成左下原点

wall_anchorX = wall_c.x/(wallInfo.max_x-wallInfo.min_x);
wall_anchorY = (wallInfo.max_y-wall_c.y)/(wallInfo.max_x-wallInfo.min_x);//物理引擎中原点在左上，这里需要换成左下原点

name1.anchorX = name1_anchorX;
name1.anchorY = name1_anchorY;

name2.anchorX = name2_anchorX;
name2.anchorY = name2_anchorX;

this.updateCallback = function(){

    var x0 = boxA.position.x;
    var y0 = boxA.position.y;


    var x1 = boxB.position.x;
    var y1 = boxB.position.y;

    name1.x = x0;
    name1.y = 1240-y0;
    name1.rotation = boxA.angle*180/Math.PI;//旋转角度矫正

    name2.x = x1;
    name2.y = 1240-y1;
    name2.rotation = boxB.angle*180/Math.PI;

}

this.scheduleUpdate();

```



## 三、多边形图形

1. 获取多边形坐标点数组
通过texturePacker工具，参数如下图：
<img width="335" alt="image" src="https://user-images.githubusercontent.com/70060430/167333337-f6bedc05-6221-48b8-a56c-75c544131556.png">

同时将高级设置中的**修剪边距**设为0，获取坐标信息放入cocos框架中

2. 处理坐标点参数
通过`Matter.Vertices.centre`获取不规则图形的重心点，然后通过坐标换算得出锚点，如下
```js
var boxA = game['addBody']('Vertices',{
    x:name1.x,
    y:1240-name1.y,
    vertexSets:ver
})


var info = game.getCenter(vertices);


var c = game['Vertices'].centre(ver);
console.log(c);//如何将重心点变为物体锚点

anchorX = c.x/(info.max_x-info.min_x);
anchorY = (info.max_y-c.y)/(info.max_x-info.min_x);//物理引擎中原点在左上，这里需要换成左下原点

console.log(anchorX,anchorY);
name1.anchorX = anchorX;
name1.anchorY = anchorY;
```

ps:使用函数`game.getCenter`时，需要放入坐标数组列表。计算锚点时注意位置换算（cocos原点是左下，matter中是左上）。
