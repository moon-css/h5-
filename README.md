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

*待补充*

## <span id = 'develop'>开发流程</span>
![h5项目开发流程图](https://user-images.githubusercontent.com/70060430/160377648-6884f878-5a2b-4a9e-a913-5aa9df8f6954.png)

### 1.前期准备
- 确认需求，确认项目技术点
- 准备合适的项目模板
- 编写接口测试用例，测试接口

### 2.图层导入
- 处理设计稿，根据命名规则和项目需求进行重命名和处理
- 按顺序运行以下三句命令，导出设计图层和设计数据
```javascript
npm run export
npm run pack
npm run export2
```

### 3.代码开发
开始编写项目代码，从加载页到内容页再到海报页。
#### 加载页
在**index.html**中的添加div，然后在**css**中添加div样式和动画，并且将加载页需要的功能用js在**index.html**中的script里实现
#### 内容页
在结束加载页的绘制后，我们在**src/base/layer**文件夹中的**TLayer.js**中进行内容页的绘制，使用**cocos2dx.js**的功能来实现图层的动画效果，以及页面的跳转。同时在**TLayer.js**文件的最后，我们有整理总结一些页面可以用到的功能函数，可以方便用户直接使用。
#### 海报页
海报页区分于加载页和内容页，是通过canvas绘制生成的一张透明的图片。用户可以通过**js**文件夹下的**game.js**中的*loadPoster*函数下载海报页图层资源，然后在下载结束之后，通过*makeResult*函数绘制canvas并且放入**index.html**中的结果容器div中。并提前在**css**下的style.css中设置好样式。

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

//css样式

#loading_number{
    font-size: 30px;
    color: rgba(255, 255, 255,0.8);
    line-height: 1.2;
    text-align: right;
    position: absolute;
    left: 246.47px;
    top: 798.28px;
    z-index: 3;
}

.sprite{
    background-image: url('../images/number.png');
}
.loading_num_0{
    width:20px;
    height:33px;
    background-position: -1px -40px
}
.loading_num_1{
    width:12px; 
    height:33px; 
    background-position: -105px -36px
}
.loading_num_2{
    width:18px; 
    height:32px;
    background-position: -85px -37px
}
.loading_num_3{
    width:18px; 
    height:33px; 
    background-position: -23px -40px
}
.loading_num_4{
    width:21px; 
    height:34px; 
    background-position: -57px -1px
}

.loading_num_5{
    width:18px; 
    height:33px; 
    background-position: -65px -37px
}

.loading_num_6{
    width:20px; 
    height:34px; 
    background-position: -80px -1px
}
.loading_num_7{
    width:17px; 
    height:33px; 
    background-position: -102px -1px
}
.loading_num_8{
    width:21px; 
    height:35px;
    background-position: -34px -1px
}
.loading_num_9{
    width:20px; 
    height:34px; 
    background-position: -43px -38px
}
.loading_num_10{
    width:31px; 
    height:37px; 
    background-position: -1px -1px;
}
#loading_percent_1{
    position: absolute;
    left: 353px;
    top: 705px;
    z-index: 2007;
}
#loading_percent_2{
    position: absolute;
    left: 376px;
    top: 705px;
    z-index: 2015;
}
#loading_percent_3{
    position: absolute;
    left: 403px;
    top: 705px;
    z-index: 2016;
}
#loading_percent_4{
    position: absolute;
    left: 433px;
    top: 705px;
    z-index: 2006;
}

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
### 2.config.js
此文件记录了所有内容页图层的数据信息，以及需要引入的js文件
```javascript
var config = {
	"basic": {
		"js": [
			{
				"name": "zepto",
				"url": "libs/zepto.min.js",
				"sync": true
			},
			{
				"name": "vconsole",
				"url": "libs/vconsole.min.js",
				"sync": true
			},
			{
				"name": "lrz",
				"url": "libs/lrz.all.bundle.js",
				"sync": false
			},
			{
				"name": "base64",
				"url": "libs/base64.min.js",
				"sync": false
			},
			{
				"name": "Sound",
				"url": "libs/Sound.min.js",
				"sync": false
			},
			{
				"name": "qrcode",
				"url": "libs/qrcode.js",
				"sync": false
			},
			{
				"name": "Video",
				"url": "libs/Video.js",
				"sync": false
			},
			{
				"name": "main0",
				"url": "./main0.js",
				"sync": false
			}
		],
		"images": [],
		"res": [
			{
				"url": "res/GameAssets_0.plist",
				"type": "plist"
			},
			{
				"url": "res/GameAssets_0.png",
				"type": "image"
			}
		],
		"videos": [],
		"scenes": [],
		"models": [],
		"textures": []
	},
	"pages": [],
	"game": {
		"scene": {
			"scene_1": {
				"z": 1,
				"rect": [
					423,
					485,
					450,
					498
				],
				"node": {
					"layer": [
						"page_1"
					]
				}
			}
		},
		"layer": {
			"page_1": {
				"z": 1,
				"rect": [
					423,
					485,
					450,
					498
				],
				"node": {
					"sprite": [
						"p_1"
					],
					"button": [
						"btn_1"
					]
				}
			}
		},
		"sprite": {
			"p_1": {
				"z": 2,
				"rect": [
					231.5,
					376.5,
					67,
					281
				]
			}
		},
		"button": {
			"btn_1": {
				"z": 1,
				"rect": [
					508,
					680.5,
					280,
					107
				]
			}
		}
	}
};
```

### 3.game.js
- 微信jssdk接口
```javascript
  getJSSDK: function (callback) {
        if(!this.isWeChat)return;
        $.get('./api/getJSSDKSign/', {
            url: window.location.href
        }, function (res) {
            callback && callback(res);
        }, 'json');
    },
```
- 跳转到cocos页面
```javascript
startScene:function(){
    var self = this;
    setTimeout(function(){
        console.log('scene_1');
        self.hidePage('loading');
        self.start('scene_1', {

        });
    },0);
},
```

- 微信分享接口
```javascript
  setShare: function (title, desc, desc2) {
        var link = h5_config.baseLink + h5_config.para;
        var imgUrl = (h5_config.baseUrl || h5_config.baseLink) + 'images/icon.jpg';

        var success1 = function () {
            //share timeline callback
            game.log('nbjc1200', "share_sussess" + "_time");
        };

        var success2 = function () {
            //share friend callback
            game.log('nbjc1200', "share_sussess" + "_friend");
        };

        var cancel = function () {
            game.log('nbjc1200', "share_sussess" + "_cancel");

        };
        wx.updateTimelineShareData({
            title: desc2, // 分享标题
            link: link, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
            imgUrl: imgUrl, // 分享图标
            success: success1,
            cancel:cancel
        });
        wx.updateAppMessageShareData({ 
            title: title, // 分享标题
            desc: desc, // 分享描述
            link: link, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
            imgUrl: imgUrl, // 分享图标
            success: success2,
            cancel: cancel
        })
    }
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
initImgs:function(resList,callback){
    this['imgList']=[];//注意清空上一次的img图片
    var loadCount=0;
    for(var i=0;i<resList.length;i++){
       var b64=resList[i];
       var img=new Image();
       img.src=b64;
       img.addEventListener('load',function(){
           loadCount++;
           if(loadCount==resList.length){
               callback();
           }
       })
       this['imgList'].push(img);
   }
},
drawImageOnPoster:function(imgList,context,leftList,topList){
    for(var i=0;i<imgList.length;i++){
        context.drawImage(imgList[i],leftList[i],topList[i]);
    }
},
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
### 4.BaseScene.js
- cocos页面适配
```javascript
   resize: function () {
        var w1 = window.innerWidth;
        var h1 = window.innerHeight;

        var w2 = cc.winSize.width;          //750
        var h2 = cc.winSize.height;         //1240

        var w3 = cc.visibleRect.width;      
        var h3 = cc.visibleRect.height;    

        var r1 = w1 / h1;
        var s = 1;
        var scale_level = -1;

        if(this.name=='scene_2'){
            if(w1 > h1){
                console.log('横');
                //var r2 = 1 / r1;
                s = w3 / 750;
            }else{
                console.log('竖');
                s = w3 / 750;
            }
    
            this.scale = s;
    
            return;
        }else{
            //////////////////////////////750x1240横竖屏适配方案
            if(w1 > h1){
                console.log('横');
                var r2 = 1 / r1;//
                if(r2 > 750 / 1030){
                    scale_level = 1;
                    s = h3 / 1030;
                }else if(r2 <= 750 / 1030 && r2 > 640 / 1030){
                    scale_level = 2;
                    s = h3 / 1030;
                }else if(r2 <= 640 / 1030 && r2 > 640 / 1240){
                    scale_level = 3;
                    s = w3 / 640;
                }else if(r2 <= 640 / 1240 && r2 > 600 / 1240){
                    scale_level = 4;
                    s = h3 / 1240;
                }else{
                    scale_level = 5;
                    s = w3 / 600;
                }
            }else{
                console.log('竖');
                if(r1 > 750 / 1030){
                    s = h3 / 1030;
                    scale_level = 1;
                }else if(r1 <= 750 / 1030 && r1 > 640 / 1030){
                    s = h3 / 1030;
                    scale_level = 2;
                }else if(r1 <= 640 / 1030 && r1 > 640 / 1240){
                    scale_level = 3;
                    s = w3 / 640;
                }else if(r1 <= 640 / 1240 && r1 > 600 / 1240){
                    scale_level = 4;
                    s = h3 / 1240;
                }else{
                    scale_level = 5;
                    s = w3 / 600;
                }
            }
            console.log(scale_level);
            this.scale = s;
        }
        
    },
```
### 5.TLayer.js
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

