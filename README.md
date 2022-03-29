# h5项目
# 项目目录
- [项目简介](#catalogue)
- [案例介绍](#examples)
- [文件目录](#docCata)
- [开发流程](#develop)
- [重点代码](#keyCode)
- [注意事项](#atten)

## <span id = 'catalogue'>项目简介</span>
h5项目是我们公司基于cocos框架以及JavaScript语言制作的一种通过浏览器实现用户交互功能的网页项目，可以实现问卷调查，知识点展示，广告营销等目的。同时此项目还可以通过不同的js库来实现不同的功能，比如视频播放，音乐切换等。
## <span id = 'examples'>案例介绍</span>
- [一个艺术节](https://m.h5in.net/ygysj/)
- [人民日报点灯](https://year2021.peopleapp.com/light)
- [芝麻文化](https://zm.zmedc.com/page/index.php)

## <span id = 'docCata'>文件目录</span>

### assets

base64  
base64_2  
bg  
clockposter  
frame  
game   
load  
loading  
loading2  
num  
num2  
other  
poster  
psd  
GameAssets.tps  
GameAssets2000.tps  
Loading.tps  
num.tps  
num2.tps  

### css
style.css
### frameworks
### images
### js
poster  
base64.js  
config.js  
coords.js  
game.js  
loader.js  
### libs
### node
### node_modules
### publish
#### 0930
css  
images  
js  
libs  
res  
#### html5
res  
index.html  
game.min.js  
project.json  
build.xml  
### res
audio  
video  
### src  
#### base 
**action**  
**label**  
**layer**  
BaseLayer.js  
TLayer.js  
**scene**  
BaseScene.js  
**sprite**  
#### unit
AnimationSprite.js  
FloatLayer.js  
MusicLayer.js  
MyFunction.js  
Shake.js  
TransparentMask.js  
Config.js  
MyLoading.js  
resource.js  

CMakeLists.txt  
manifest.webapp  
yarn.lock  
index.html  
.cocos-project.json  
gulpfile.js  
main.js  
main0.js  
package-lock.json  
package.json  
project.json  
report.20210518.123525.7096.0.001.json


## <span id = 'develop'>开发流程</span>
![h5项目开发流程图](https://user-images.githubusercontent.com/70060430/160612180-f9123954-81cc-4a87-a212-dba6c75205e5.jpg)

### 1.前期准备
- 确认需求，确认项目技术点
- 准备合适的项目模板
- 编写接口测试用例，测试接口

### 2.图层导入
- 处理设计稿，根据命名规则和项目需求进行重命名和处理,处理好的设计图命名为**game.psd**放在**assets/psd**中
- 按顺序运行以下三句命令，导出设计图层和设计数据
```javascript
npm run export
npm run pack
npm run export2
```

### 3.代码开发
开始编写项目代码，从加载页到内容页再到海报页。
#### 加载页

代码示例：

```javascript

index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
    <title>页面标题</title>
    <link type="text/css" rel="stylesheet" href="css/style.css">
</head>
<body>
    <div id="root">
        <div id="loading">
    		<!--加载页元素-->	
        </div>
    </div>
    <!--引入文件-->
    
    <script src="libs/Stage.min.js"></script>
    <script>
        var h5_config = {
            baseUrl: '',
            baseLink: '正式链接',
            para: '',
            key:'abc123',
            ukey:'user123',
            startTime:new Date().getTime(),
            wxConfig: {
                // 配置信息, 即使不正确也能使用 wx.ready
                debug: false,
                appId: '',
                timestamp: 1,
                nonceStr: '',
                signature: '',
                jsApiList: []
            },
            nickname:'昵称',
	    headImgUrl:'',
            visitCount:10330,
            titleArr: [点对点分享标题],
            descArr: [点对点分享描述],
            descArr2: [朋友圈分享标题],
            loadJs: function (url,callback) {
                var script = document.createElement("script");
                script.src = url;
                document.body.appendChild(script);
                script.addEventListener("load", function () {
                    script.parentNode.removeChild(script);
                    callback && callback();
                    this.removeEventListener('load', arguments.callee, false);
                });
            }
        };
        var video2;
        var act;
        var video_radio = 1240 / 750;
        var stage = new Stage('root', {
            width: 640,
            height: 1240,
            designWidth: 750,
            designHeight: 1240,
            safeWidth: 600,
            safeHeight: 1030,
            zIndex: 1001
        });
        // stage.show();
        stage.setResizeCallback(function () {
            var offsetX = (stage.w - stage.width) / 2;
        });
	//加载进度
        function updateProgress(progress) {
            var number = Math.floor(progress * 100);
            document.getElementById('loading').innerHTML = "加载进度：" + number + "%";//loading替换为自己定义的加载进度数字的div的id名
        }
        updateProgress(0);
        window.onload = function () {
            setTimeout(function () {
                stage.show();
                var loading = document.getElementById('loading');
                loading.style.opacity = '1';
            }, 300);
            h5_config.loadJs('js/game.js',function(){
                h5_config.loadJs('js/loader.js');
            });
        };
    </script>
    <script src="frameworks/cocos2d-html5/CCBoot.js"></script>
</body>
</html>


game.js

//如果需要点击开始的话，首先把game.js中的所有startScene函数调用注释掉，然后在initlayer中编写以下代码;如果加载结束直接开始的话就不需要写开始点击代码

initLayer: function () {
	var self = this;
	$("#start").on('touchend',function(){//start为开始按钮的id名
		game.startScene();
	)
},
```
#### 内容页

代码示例：
```javascript
TLayer.js

case '页面名称':
	console.log('page_1');
	//绘制内容页
break;

```
页面名称和**game.psd**中处理的每一页的名称对应。并且如果出现多个页面都需要使用的功能函数，最好该函数定义在**game.js**或者命名为game对象中的函数。



#### 海报页
代码示例：
```javascript
TLayer.js

game['loadPoster'](game['num'],function(){//game['num']是指需要生成的海报参数，如果没有多种海报的话，把loadPoster函数中的这个参数删掉就可以了
    game['makeResult'](function(){
    var result_background = new TLayer('page_result');
    self.parent.addChild(result_background, self.zIndex + 1);
                            
    })
})
```
调用game中的两个函数生成海报，在生成之后再跳转到结果页。**loadPoster，makeResult**两个函数具体看[*重点代码*](#keyCode)中的game.js块。


### 4.本地测试
所有的页面绘制结束之后，打开本地地址，在手机以及电脑上本地预览，测试有无bug，并且优化整体代码。

### 5.文件编译
当本地测试没有问题时，运行命令`npm run compile`编译文件，生成**game.min.js**。

### 6.打包cdn并上传服务器
运行命令`node node/update_files.js`打包cdn,此时cdn文件在**publish/0930**文件夹中，**0930**是自己创建的文件夹，可以自己命名。但是需要同时在**node**文件夹中的update_files.js文件中修改文件夹名，如下图中圈出的地方：
<img width="493" alt="image" src="https://user-images.githubusercontent.com/70060430/160553531-2fddf2f7-1f0c-4589-bf7c-a27da6235af5.png">

### 7.线上测试
当代码包上传到服务器上后，我们就可以在指定浏览器端进行线上测试，调试bug，直至没有问题后，将项目正式链接提交给客户进行验收。
## <span id = 'keyCode'>重点代码</span>

### 1.index.html、style.css
- 文件库引入
<img width="608" alt="image" src="https://user-images.githubusercontent.com/70060430/160554621-f1083e52-0309-47ab-9fd8-5e3fae107e63.png">

- 配置初始信息
```javascript
 var h5_config = {
    baseUrl: '',
    baseLink: '正式链接地址',//手动写入
    para: '',
    fr:0,
    key:'abc123',
    ukey:'user123',
    startTime:new Date().getTime(),
    wxConfig: {
        // 配置信息, 即使不正确也能使用 wx.ready
        debug: false,
        appId: '',
        timestamp: 1,
        nonceStr: '',
        signature: '',
        jsApiList: []
    },
    nickName:'昵称昵称昵称昵称昵称',
    headImgUrl
    visitCount:10330,
    titleArr: ['点对点分享标题'],//手动写入
    descArr: ['点对点分享描述'],//手动写入
    descArr2: ['盆友圈分享标题'],//手动写入
    loadJs: function (url,callback) {
        var script = document.createElement("script");
        script.src = url;
        document.body.appendChild(script);
        script.addEventListener("load", function () {
            script.parentNode.removeChild(script);
            callback && callback();
            this.removeEventListener('load', arguments.callee, false);
        });
    }
};
```

- 进度条显示
自定义样式进度数字图层：
```javascript
//div设置

<div id="loading_percent" class="loading">
    <div id="loading_percent_1" class="sprite"></div>
    <div id="loading_percent_2" class="sprite loading_num_0"></div>
    <div id="loading_percent_3" class="sprite loading_num_10"></div>
    <div id="loading_percent_4" class="sprite"></div>
</div>

//js代码
function updateProgress(progress,callback) {
    var percent = Math.floor(progress * 100);

    var str = String(percent);
    var arr = ['', '', '', ''];//个十百位数字加上最后的百分号

    //以右侧百分号为位置准线，loading_num_10是百分号，通过改变dom元素的className来改变进度数字
    switch (str.length) {
        case 1:
            arr = [' loading_num_' + str, ' loading_num_10',''];
            break;
        case 2:
            arr = [' loading_num_' + str[0], ' loading_num_' + str[1], ' loading_num_10',''];
            break;
        case 3:
            arr = [' loading_num_' + str[0], ' loading_num_' + str[1], ' loading_num_' + str[2], ' loading_num_10'];
            break;
    }

    document.getElementById('loading_percent_1').className = 'sprite' + (arr[0] || '');
    document.getElementById('loading_percent_2').className = 'sprite' + (arr[1] || '');
    document.getElementById('loading_percent_3').className = 'sprite' + (arr[2] || '');
    document.getElementById('loading_percent_4').className = 'sprite' + (arr[3] || '');
    // document.getElementById('progress').style.width=1.14*progress+'px';
}
```
系统样式进度数字：

```javascript
function updateProgress(progress) {
    var number = Math.floor(progress * 100);
    document.getElementById('loading').innerHTML = "加载进度：" + number + "%";//此处的loading可以改为自己定义的进度数字div的id名
}

```

### 2.game.js
- 跳转到cocos页面，可以控制进入具体那个scene里
```javascript
startScene:function(){
    var self = this;
    setTimeout(function(){
        console.log('scene_1');
        self.hidePage('loading');
        self.start('scene_1', {//如果设置了多个scene，此处控制从loading页第一次进入的scene

        });
    },0);
},
```

- 加载海报页资源
```javascript
loadPoster:function(name,callback){
	var load_count=0;
	var self=this;
	console.log(game['num']);
	var loadList=['js/coords.js','js/poster/rs_'+name+'.js'];//生成海报页所需的js文件

	for(var i=0;i<loadList.length;i++){
	    loader.loadScript(loadList[i],function(){
		load_count++;
		if(load_count==loadList.length){
		    var base64List=[
			game['poster']['poster'],
			game['poster']['share']
		    ];
		    self.initImgs(base64List,function(){//初始化图层
			callback && callback();
		    })
		}
	    })
	}
},
```
- 生成海报
```javascript
makeResult: function (callback) {
    var self=this;
    var leftList=[config_coords['rs_'+(game['num'])]['left'],config_coords.share.left];
    var topList=[config_coords['rs_'+(game['num'])]['top'],config_coords.share.top];
    console.log(game['num']);

    var canvas=document.createElement('canvas');
    canvas.width=750;
    canvas.height=1450;
    var ctx=canvas.getContext('2d');

    ctx.clearRect(0,0,canvas.width,canvas.height);

    var result=document.getElementById('result');
    if(result){
        result.remove();
    }
    
    self.drawImageOnPoster(game['imgList'],ctx,leftList,topList);
    console.log(game['imgList']);
    //还可以在canvas上绘制文字和其他图片资源，且一般图片资源用base64格式比较合适

    var data = canvas.toDataURL('image/png');
    var _img = new Image();
    _img.src = data;
    _img.id = "result";

    var result = document.getElementById('result');
    var container_result = document.getElementById('container_result');
    if (!result) {
        result = new Image();
        result.id = 'result';

        result.addEventListener('touchmove', function(e) {
            e.preventDefault();
        }, {
            passive: false
        });
        result.src = data;

        container_result.appendChild(result);
        self.showPage('container_result');
        callback && callback(data); //回调函数
    }

},
```

### 4.TLayer.js
- 翻页
```javascript
self.createAndFadeOut(self,'目标页名')
```

- 点击
```javascript
this.getChildByName('按钮名称').setCallback(callbackEnd,callbackMove,callbackStart)
```

- 动画
```javascript
item.fadeIn(time)//元素出现

item.fadeOut(time)//元素消失

item.moveTo(time,x,y)//元素在time内移动到（x,y）

item.moveBy(time,x,y)//元素在time内向右移动x，向下移动y

item.scaleTo(time,s)//元素在time内缩到s大小

```

## <span id = 'atten'>注意事项</span>
- **main.js**是项目在服务器上运行时的代码，**main0.js**是项目在本地运行时的代码，所以当涉及到这两个js文件的修改时
，需要同时修改两个文件内容

-上传cdn之前，我们需要把**publish/html5**中的**game.min.js**文件复制到cdn所在的*js*文件夹中

