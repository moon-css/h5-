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

### 1.index.html

### 2.style.css

### 3.config.js

### 4.game.js

### 5.TLayer.js

### 6.main.js、main0.js


## <span id = 'atten'>注意事项</span>
