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
用phtotshop处理设计稿，根据命名规则和项目需求进行重命名和处理，处理好的源文件命名为**game.psd**放在**assets/psd**中，然后按顺序在命令行工具中执行以下脚本：

- Step1:运行`num run export`切图脚本，将psd文件切成待处理png/jpg格式图片。切图成功后，在**assets/game**文件夹中可以看到对应的图层文件。
- Step2:运行`npm run pack`脚本，调用TexturePacker命令行工具，将切好的图片组打包成精灵图。运行成功后，在**res**文件夹下可以看到若干拼好的精灵图
- Step3:运行`'npm run export2'脚本，将图层的位置/页面等信息写入**config.js**文件中。

### 3.代码开发
在代码编程工具中开始编写项目代码，从加载页到内容页再到海报页依次开始绘制。

#### 1）加载页
##### index.html
- 更改页面名称
```html
 <title>页面名称</title>
```
- 在下面代码loading内添加div元素
```html 
<div id="root">
    <div id="loading">
            
    </div>
</div>
```
- 将*h5_config*对象中的baseLink更改为当前项目的正式链接
- 在*h5_config*对象中，将分享文案填入
```javascript
var h5_config = {
    baseUrl: '',
    baseLink: '正式上线地址',
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
    nickname:'昵称昵称昵称昵称昵称',
    visitCount:10330,
    titleArr: ['点对点分享标题'],
    descArr: ['点对点分享描述'],
    descArr2: ['朋友圈分享标题'],
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
##### game.js
- 在initlayer函数中写入在需要在运行**index.html**之后，加载结束之前运行生效的代码

#### 2）内容页

##### game.js
- 编写接口函数
- 编写功能性函数，如播放视频，播放声音等

##### TLayer.js
- 代码示例：
```javascript
case '页面名称':
	console.log('page_1');
	//绘制内容页
break;

```
页面名称和**game.psd**中处理的每一页的名称对应。

#### 3）海报页
##### game.js
- 根据海报页需求，修改*loadPoster*函数中的参数name和下载文件列表对象loadList
```javascript
loadPoster:function(name,callback){
	var load_count=0;
	var self=this;
	console.log(game['num']);
	var loadList=['js/coords.js','js/poster/rs_'+name+'.js'];

	for(var i=0;i<loadList.length;i++){
	    loader.loadScript(loadList[i],function(){
		load_count++;
		if(load_count==loadList.length){
		    var base64List=[
			game['poster']['poster'],
			game['poster']['share']
		    ];
		    self.initImgs(base64List,function(){
			callback && callback();
		    })
		}
	    })
	}
},
```

- 根据海报页内容，修改*makeResult*函数中canvas的尺寸，绘制的内容，如需要额外绘制文字等
```jacascript
 makeResult: function (callback) {
    var self=this;
    var leftList=[config_coords['rs_'+(game['num'])]['left'],config_coords.share.left];
    var topList=[config_coords['rs_'+(game['num'])]['top'],config_coords.share.top];//config_coords是自己处理获得的海报页所有元素的位置信息
    var canvas=document.createElement('canvas');
    canvas.width=750;//canvas的宽
    canvas.height=1450;//canvas的高
    var ctx=canvas.getContext('2d');
    ctx.clearRect(0,0,canvas.width,canvas.height);

    var result=document.getElementById('result');
    if(result){
        result.remove();
    }
    
    self.drawImageOnPoster(game['imgList'],ctx,leftList,topList);
    console.log(game['imgList']);
    //如需绘制自定义文字和图片请在此处开始绘制
  
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
##### TLayer.js
- 通过调用海报绘制函数，生成海报并跳转进入结果页
代码示例：
```javascript
game['loadPoster'](game['num'],function(){//game['num']是指需要生成的海报参数，如果没有多种海报的话，把loadPoster函数中的这个参数删掉就可以了
    game['makeResult'](function(){
    var result_background = new TLayer('page_result');
    self.parent.addChild(result_background, self.zIndex + 1);
                            
    })
})
```


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

## <span id = 'atten'>注意事项</span>
- **main.js**是项目在服务器上运行时的代码，**main0.js**是项目在本地运行时的代码，所以当涉及到这两个js文件的修改时
，需要同时修改两个文件内容

- 上传cdn之前，我们需要把**publish/html5**中的**game.min.js**文件复制到cdn所在的*js*文件夹中

