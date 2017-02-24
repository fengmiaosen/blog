---
title: HTML5游戏引擎对比分析调研
date: 2016-04-26 15:06:06
tags: [html5,游戏]
categories: 
    - html5
comments: true
toc: true  //在此处设定是否开启目录，需要主题支持
---

> 选取了国内外3款比较有代表性的游戏开发引擎（**[Hilo](http://hiloteam.github.io/index.html)、[Egret](http://edn.egret.com/cn/)、[Phaser](http://phaser.io/)**），从`引擎体积`、`渲染性能`、`学习开发成本`、`功能特性`、`开发入门`、`辅助开发工具`、`社区支持`、`商业应用`、`设计思想`几个维度进行比较，分析各个引擎框架的优缺点以及各自的适用场景，为日常项目中游戏开发提供有价值的参考资料。

![分析对比](http://7xt6po.com1.z0.glb.clouddn.com/blog/%E6%B8%B8%E6%88%8F%E5%BC%95%E6%93%8E.png)

<!-- more -->

# 引擎体积
* 引擎比较

游戏引擎           | 核心库(KB)|  核心库GZip(KB)|骨骼动画引擎(KB)|物理引擎(KB)
------------------|----------|--------------|----------------|------------
Hilo (Standalone) |   65  | 24 |66 | 64（Chipmunk引擎）
Hilo (AMD)        |   67  | 24 |66 | 64
Hilo (CMD)        |   66  | 24 |66 | 64
Egret             | 300 | 80  |66  | 119 (P2引擎)
Phaser            | 348 | 173 |无 | 215 (Arcade引擎)

![体积比较](http://7xt6po.com1.z0.glb.clouddn.com/blog/diff.png)

* Hilo和Egret都基于开源2D骨骼动画解决方案 [`DragonBones`](http://dragonbones.github.io/) 来实现骨骼动画。
* Phaser目前暂时没有内置的骨骼动画引擎，如有需要可引入开源骨骼动画框架，[点击查看](http://www.html5gamedevs.com/topic/6666-phaser-dragonbones/)。

# 功能特性
* 功能特性对比表

特性\引擎| Hilo  | Egret  | Phaser 
-------|--------|------|------
Canvas | Yes    | Yes   | Yes
WebGL  | Yes    | Yes   | Yes 
Flash渲染  | Yes   |  `No`   |   `No`
DOM渲染 | Yes   | `No` |  `No`
矢量绘图 | Yes   | Yes    | Yes
3D渲染 | `No`   | Yes |  `No`
preload资源预加载 | yes **   | yes****   | yes****
audio/sound | yes | yes | yes
device scale | `No` | Yes | Yes
Button/Input/event | yes | yes | yes
Bitmap/text/纹理 | yes | yes | yes
group特性 | `No` | `No` | Yes
camera相机特性| Yes | Yes | Yes
physics物理特性 | Yes *** | Yes**** | Yes****
Particle粒子特性| Yes *** | `No` | Yes ****
骨骼动画 | Yes **** | Yes **** | Yes ****
Tween/Animation  | Yes | Yes | Yes
UI组件库 | `No` | Yes | `No`
native app | `No` | Yes | `No`
IDE工具 | `No`未开源 | Yes | `No`
Plugin | Yes ** | Yes **** | Yes ****


* **Hilo** 是由阿里巴巴研发并开源的一个跨终端的互动小游戏解决方案，内核精简，功能全面。
    * 提供DOM，Canvas，Flash，WebGL等多种渲染方案。
    * 物理引擎支持——Chipmunk，支持自扩展物理实现；
    * 骨骼动画支持——[`DragonBones`](http://dragonbones.github.io/)，同时内建骨骼动画系统——Tahiti（目前内部使用）
    * 集成 Hilo Audio, Hilo Preload 等游戏构建服务的综合解决方案以及动画纹理构建方案。
    
    ![Hilo功能特性](http://sm01.alicdn.com/L1/272/6837/static/dongxiao_topic/image/Hilo功能特性.png)
    
* **[Egret](http://edn.egret.com/cn/)** 是一整套游戏开发的解决方案，包括游戏开发框架、开发辅助工具。
    * 基于[TypeScript](http://www.typescriptlang.org/)进行开发，支持Flash到Egret的高效转换
    * 跨平台支持，可打包为`HTML5`，以及`iOS` 和 `Android`、`Windows Phone`（貌似没啥必要）平台原生app，极大提高游戏性能。 [点击查看](http://edn.egret.com/cn/docs/page/649)
    * 接入[Egret Runtime](http://www.egret.com/products-runtime)，可彻底解决HTML5游戏在WebView中的卡顿问题
    * 提供核心UI扩展库 `EUI`，[EUI](http://edn.egret.com/cn/docs/page/518)用 EXML 来开发应用界面，可基于已有组件进行组合或扩展
    * EUI 组件库
      ![EUI扩展库类图](http://sedn.egret.com/560200b50f195.png)
      
    * 功能特性图
        ![Egret功能特性](http://sm01.alicdn.com/L1/272/6837/static/dongxiao_topic/image/Egret功能特性.png)

* **[Phaser](http://phaser.io/)** 是一款专门用于HTML5 2D游戏开发的开源框架，提供JavaScript和TypeScript双重支持，内置游戏对象的物理属性，采用[Pixi.js](http://www.pixijs.com/)渲染引擎以加快Canvas和WebGL渲染

    * 功能特性
    ![Phaser功能特性](http://sm01.alicdn.com/L1/272/6837/static/dongxiao_topic/image/phaser功能特性.png)

# 渲染性能
> 主要测试同时渲染大量位图时的性能表现，分别测试在渲染1000个位图对象时的动画实时帧数（FPS）
    
* [hilo-test](http://fms126.github.io/hilo-test/)
* [phaser-test](http://fms126.github.io/phaser-test/)
* [egret-test](http://fms126.github.io/egret-test/)

# 开发入门

   引擎 | 构建工具 | 可扩展模块 | 开发语言 | 入门成本 | 开发成本
-------|----------|-----------|------|----------|----------
Hilo   | Gulp && [yeoman](https://github.com/yeoman/yo)  | standalone、AMD、CMD、Kissy、flashshim | JavaScript | 中 | 高
Egret  | EgretWing&命令行| Game、Tween、WebSocket、p2、ParticleSystem | TypeScript | 高 | 中
Phaser | Grunt | ninja、 p2、 particles、gamepad、 keyboard... | JavaScript、TypeScript | 中 | 中

# 辅助工具

* Hilo
    * 骨骼动画系统——Tahiti（目前内部使用）
* Egret
    * 可视化IDE —— [Egret Wing](http://www.egret.com/products/wing.html)  
    ![EgretWing](http://www.egret.com/img/products/wing/wanzheng.gif)
    * 3D引擎 —— [Egret Engine 3D](http://edn.egret.com/cn/docs/page/775)
    * 基于XML语法扩展的组件库方案 —— [EUI](http://edn.egret.com/cn/docs/page/666)
    * 骨骼动画解决方案（编辑器、插件、基础库）—— [DragonBones](http://edn.egret.com/cn/docs/page/364)
    ![dragonbones](http://sedn.egret.com/ueditor/20150727/55b5e611db7e2.png)
    * 无需编程的免费游戏创作工具 —— [Lakeshore](http://edn.egret.com/cn/docs/page/476)
    * 其它辅助工具——（[egret-iOS-support](http://edn.egret.com/cn/docs/page/236)、[Egret Android Support](http://edn.egret.com/cn/docs/page/235)、[Egret Inspector](http://edn.egret.com/cn/docs/page/240)、[TS Conversion](http://edn.egret.com/cn/docs/page/233))
* Phaser
    * 在线代码调试编辑 —— [phaser sandbox](http://phaser.io/sandbox/edit/1)
    * API链式方法在线查询 —— http://phaser.io/learn/chains
    * 第三方商业骨骼动画解决方案 
        * [creature](http://creature.kestrelmoon.com/more_info_1.html)
        * [Advance 2D Skeletal + Mesh Animation Tool now with Phasor Support](http://www.html5gamedevs.com/topic/13290-creature-advance-2d-skeletal-mesh-animation-tool-now-with-phasor-support/)

# 社区支持

  引擎  | 开源  | 文档支持| 示例 | API搜索|社区支持| 第三方插件 | 语言支持
-------|-------|-------|------|------|----------- |-------|-------
Hilo   | 部分开源|中文文档 | [少量示例](http://hiloteam.github.io/examples/index.html)| 不支持 | github | 无 | 中文
Egret  | 完全开源 |丰富的中文文档 | [大量丰富的示例](http://edn.egret.com/cn/list/example/) | 支持 |github&[专业论坛社区](http://bbs.egret.com/forum.php) | 开发平台支持，免费为主 | 中文
Phaser | 完全开源 |丰富的英文文档 | [大量丰富的示例](http://phaser.io/examples)| 支持 |  github&[专业论坛社区](http://www.html5gamedevs.com/) | 收费的[商业插件](http://phaser.io/shop)为主 | 英文

# 商业应用

* 国内应用：
    
    国内有人用了国内比较火的HTML5游戏平台新浪微博作为数据采样基础，判断游戏到底使用什么引擎制作，[点这里](http://gold.xitu.io/entry/5710a800ebcb7d00559448a5)。最终统计结果如下：
![game-app](http://7xt6po.com1.z0.glb.clouddn.com/blog/game-app.png)

* 国外排行榜：
[html5 game engine](https://html5gameengine.com/)
![h5 game](http://7xt6po.com1.z0.glb.clouddn.com/blog/h5-game.png)


# 游戏开发通用模式

![HTML5游戏通用开发模式](http://sm01.alicdn.com/L1/272/6837/static/dongxiao_topic/image/HTML5游戏通用开发模式.png)

* 游戏舞台、对象容器、场景（game、stage）
    * 舞台场景是所有游戏对象的载体和容器
    * Hilo.Stage 舞台
    
    ```js
    var stage = new Hilo.Stage({
    renderType:'canvas',
    container: containerElem,
    width: 320,
    height: 480
    });
    
    ```
    * egret 游戏对象容器
    
    ``` js
   this.gameLayer = new egret.DisplayObjectContainer();
   this.addChild(this.gameLayer);  
       
    ```
    * Phaser 游戏舞台对象
    
    ```js
    // 状态管理，详情参照 http://www.iphaser.cn/learn/stateManager
    var states = {
        preload: function(){},
        create: function(){},
        update: function(){}
    };
    var game = new Phaser.Game(gameWidth, gameHeight, Phaser.CANVAS, 'wrapper', states , true);
    ```
* 游戏可视化对象（Sprite、bitmap、text）
    * 具有独立功能的独立显示对象，包括位图对象、文本对象、矢量图形、可视化对象群组等
    * hilo 可视对象(位图对象Bitmap)
    
    ```js       
    var bird = new Hilo.Bitmap({
    image: 'images/bird.png'
    }).addTo(stage);
    ```
    * egret 可视对象
    
    ```js       
   var bitmap:egret.Bitmap = new egret.Bitmap();
   bitmap.texture = RES.getRes("bgImage");
   this.gameLayer.addChild(bitmap);  
    ```
    * phaser 可视对象

    ```js       
     game.load.image('pic', 'assets/pics/acryl_bladerunner.png');
     var image = game.add.image(100, 100, 'pic');  
    ```
* 游戏资源加载（preload、res）
    * 加载图片、json文件、音视频资源等
    * hilo 预加载资源
    
    ```js       
        var queue = new Hilo.LoadQueue();  
        queue.add({id:'bg', src:'images/bg.png'});
        queue.on('load', function(e){
        //console.log('load:', e.detail);
        });
        queue.on('error', function(e){
        //console.log('error:', e.detail);
        });
    ```

    * egret 预加载资源
        * 资源配置json文件
        
        ```json
            {
               "name":"mc_png",
               "type":"image",
               "url":"assets/mc.png"
            },
            {
                "name":"big_json",
                "type":"json",
                "url":"assets/big.json"
            }
        ```
        * 资源加载解析器
        
        ```js       
            RES.registerAnalyzer(type:image, analyzerClass:any);
            
            // 同步获取缓存资源
            var texture:egret.Texture = RES.getRes("mc_png");
            
            // 异步获取资源
            RES.getResAsync("mc_png", function(res){
            
            });
            
            // 加载资源组，支持回调函数
            RES.addEventListener( RES.ResourceEvent.GROUP_COMPLETE, this.onResourceLoadComplete, this );
            RES.addEventListener( RES.ResourceEvent.GROUP_PROGRESS, this.onResourceProgress, this );
            RES.addEventListener( RES.ResourceEvent.GROUP_LOAD_ERROR, this.onResourceLoadErr, this );
            RES.loadGroup( "preload" );
        ```
    * Phaser 资源加载
    
    ```js
    function preload() {
        game.load.image('mushroom', 'assets/sprites/mushroom2.png');
        game.load.audio('boden', ['rock_4bit.mp3']);
        game.load.json('version', 'http://phaser.io/version.json');
    }
    function create() {
        var test = game.add.sprite(200, 200, 'mushroom');
        var music = game.sound.play('boden');
        var phaserJSON = game.cache.getJSON('version');
    }
    ```
    
* 动画特性支持（tween、animation）
    * Hilo 动画 Tween
    
    ```js
        //循环移动地面
        Hilo.Tween.to(this.ground, {x:-60}, {duration:300, loop:true});
    ```
    * egret动画 Tween
    
    ```js
        egret.Tween.get(this.jian_png, { loop: isloop }).to(obj, duration, ease).wait(waited);
    ```
    * phaser动画 Tween
    
    ```js
        var bounce=game.add.tween(ball);
        bounce.to({ y: game.world.height-ball.height }, 1000 + Math.random() * 3000,Phaser.Easing.Bounce.In);
        bounce.onComplete.add(startBounceTween, this);
        bounce.start();
    ```
* 用户交互处理（input、event）
    * 提供一套事件机制，支持用户通过各种方式的输入，包括鼠标、键盘、触摸、重力感应等
    * hilo 事件交互
    
    ```js
    stage.enableDOMEvent(Hilo.event.POINTER_START, true);
    sprite.on(Hilo.event.POINTER_START, function(e){
        console.log(e.eventTarget, e.stageX, e.stageY);
    });
    ```
    * egret事件交互
    
    ```js
        class Main extends egret.DisplayObjectContainer {       
            public constructor() {
                super();
            this.addEventListener(egret.Event.ADDED_TO_STAGE,this.onAddToStage, this);
            }
        
            private onAddToStage(event:egret.Event) {
                // 触摸事件
                var spr1:egret.Sprite = new egret.Sprite();
                spr1.addEventListener( egret.TouchEvent.TOUCH_TAP, this.onTouch, this );
            }
        }
    
    ```
    * phaser事件交互
    
    ```js
         // 键盘事件
        if (game.input.keyboard.isDown(Phaser.Keyboard.LEFT))
        {
        }
    ```
* 屏幕适配（scale、rotate）
    * 提供舞台宽高和屏幕尺寸之间的适配模式（showAll、exactFit等）
    * egret屏幕适配
    [点击查看API](http://edn.egret.com/cn/apidoc/index/name/egret.StageScaleMode)
    
    ```js
    //保持原始宽高比缩放应用程序内容，缩放后应用程序内容的较宽方向填满播放器视口，另一个方向的两侧可能会不够宽而留有黑边。
    this.stage.scaleMode = egret.StageScaleMode.SHOW_ALL;
    this.stage.orientation = egret.OrientationMode.AUTO;
    ```
    * phaser 屏幕适配
    
    ```js
      // 游戏居中
   game.scale.pageAlignHorizontally = true;
   game.scale.pageAlignVertically = true;
   // 适应父元素的大小，但会保持游戏的宽高比例
   game.scale.scaleMode = Phaser.ScaleManager.SHOW_ALL;
    ```
* 音频、视频处理（Video、Sound）
    * 提供音频视频媒体资源的播放处理
    * Egret音视频
    
    ```js
        var sound:egret.Sound = new egret.Sound();
        sound.addEventListener(egret.Event.COMPLETE, function loadOver(event:egret.Event) {
          sound.play();
      }, this);
      
      this.video = new egret.Video();
      this.video.load("http://media.w3.org/2010/05/sintel/trailer.mp4");
      //监听视频加载完成
      this.video.once(egret.Event.COMPLETE,this.onLoad,this);
    ```
    * phaser音视频
    
    ```js
        //预加载音频资源
        game.load.audio('sfx', 'assets/audio/SoundEffects/fx_mixdown.ogg');
        
        //加载完毕在回调函数(creat函数)中初始化
        var fx = game.add.audio('sfx'); 
        fx.play();
    ```
* 游戏场景刷新、定时器（update、ticker）
    * 控制游戏舞台按照一定帧率（60FPS）来不断刷新
    * 提供定时器对象 time ticker
    * hilo 定时器
    
    ```js
    //舞台Stage上的物体的运动等变化，都是通过一个定时器Ticker不断地调用Stage.tick()方法来实现刷新的。
    var ticker = new Hilo.Ticker(60);
    ticker.addTick(stage);
    ticker.start();
    ```
    * egret计时器
    
    ```js
        //创建一个计时器对象
       var timer:egret.Timer = new egret.Timer(500,5);
       //注册事件侦听器
       timer.addEventListener(egret.TimerEvent.TIMER,this.timerFunc,this);
        timer.addEventListener(egret.TimerEvent.TIMER_COMPLETE,this.timerComFunc,this);
       //开始计时
       timer.start();
    ```
    * phaser计时器
    
    ```js
     game.time.events.loop(Phaser.Timer.SECOND, updateCounter, this);
     
     function updateCounter() {
        counter++;
    }
    ```
    
* 物理特性、粒子特性支持
    * 支持碰撞检测等物理特性
    * egret 物理特性
    
    ```js
         //创建world
        var world:p2.World = new p2.World();
        world.sleepMode = p2.World.BODY_SLEEPING;      
        ......     
        var boxBody:p2.Body = world.bodies[i];
        var box:egret.DisplayObject = boxBody.displays[0];
          
        //添加方形刚体
        var boxShape:p2.Shape = new p2.Rectangle(2, 1);
        var boxBody:p2.Body = new p2.Body({ mass: 1, position: [positionX, positionY], angularVelocity: 1});
        boxBody.addShape(boxShape);
        world.addBody(boxBody);
    ```
    * phaser 物理特性
    
    ```js
        var image = game.add.sprite(0, 0, 'flyer');
        game.physics.enable(image, Phaser.Physics.ARCADE);
        // 设置对象速度
        image.body.velocity.setTo(200,200);
        // 设置是否检测碰撞
        image.body.collideWorldBounds = true;
    ```

* 骨骼动画支持
    * 使用一套图片资源，组合创造骨骼动画
    * hilo 骨骼动画 [点击查看](http://g.alicdn.com/tmapp/hilodemos/3.0.7/mv1111/index.html)
    
    ![骨骼动画示例](http://7xt6po.com2.z0.glb.clouddn.com/blog/hilo-d.gif)
        
    * Egret 骨骼动画[点击查看](http://edn.egret.com/cn/article/index/id/33)
    
    ![手机访问](http://7xt6po.com2.z0.glb.clouddn.com/blog/egret%E9%AA%A8%E9%AA%BC%E5%8A%A8%E7%94%BB.png)
    
    * Phaser骨骼动画 [点击查看](http://phaser.io/examples/v2/animation/creature-dragon)
    
    ![phaser gif](http://7xt6po.com1.z0.glb.clouddn.com/blog/phaser.gif)
    
* 多重渲染模式支持（canvas、webgl、flash）
    * 针对不同的终端以及浏览器环境，自动选择渲染模式，屏蔽差异
    * egret 支持canvas和WebGL渲染
    
    ```js
    egret.runEgret({renderMode:"canvas"});
    
    egret.runEgret({renderMode:"webgl"});
    ```
    * phaser 支持自动选择渲染模式（WebGL>Canvas）或者手动选择
    
    ```js
    var states = {};
    //让游戏引擎根据浏览器特性自动选择
    var game = new Phaser.Game(gameWidth, gameHeight, Phaser.AUTO, 'wrapper', states , true);
    
    var game = new Phaser.Game(gameWidth, gameHeight, Phaser.CANVAS, 'wrapper', states , true);
        
    var game = new Phaser.Game(gameWidth, gameHeight, Phaser.WEBGL, 'wrapper', states , true);
    ```
    
* 可扩展模块
    * 支持模块化配置，可选择引入需要的功能模块，减小基础库体积
    * 支持第三方插件无缝接入
    * [Egret第三方库](http://edn.egret.com/cn/docs/page/172)
        * 一些非核心内容(比如 MovieClip)都移到了扩展模块中，通过配置文件 `egretProperties.json`指定扩展模块
        * Egret官方库按照模块呈现，避免加载不需要的模块，减少最终代码的体积
        
            ```
            egret 必备的核心库
            game 制作游戏会用到的类库，比如 MovieClip，URLLoader 等
            res 资源加载库，所有涉及资源载入的工作，都可以通过这个模块来完成
            tween 动画缓动类
            dragonbones 龙骨动画库，用来制作一些复杂的动画效果
            socket 用来通讯的 WebSocket 库
            gui 老版本的 UI 库
            eui 新增的 UI 库，使用起来更加的方便
            ```
        * 标准第三方库可以是标准的 ts 库，也可是你在网上下载现成的 js 库经过扩展来使用 [详情请看](http://edn.egret.com/cn/docs/page/172)
    * [Phaser第三方插件](http://phaser.io/shop)
        * 主要为商业收费插件，功能强大，提供完善的文档和示例，例如：[Box2D物理引擎插件](http://phaser.io/examples/v2/category/box2d)
        * 基于Phaser的plugin机制来使用第三方插件，[插件参照](http://phaser.io/shop/plugins)   

# 参考资料
* [阿里自主研发的互动游戏引擎Hilo开源啦](http://www.atatech.org/articles/50174)
* [手把手教你用Hilo开发一个小游戏](http://www.atatech.org/articles/15940)
* [Hilo官方网站](http://hiloteam.github.io/index.html)
* [Egret Engine](http://edn.egret.com/cn/docs/page/579)
* [Egret官方API](http://edn.egret.com/cn/apidoc)
* [Egret官方示例](http://edn.egret.com/cn/list/example/)
* [Phaser](http://phaser.io/)
* [Phaser官方示例](http://phaser.io/examples)
* [Phaser官方入门教程](http://phaser.io/tutorials/making-your-first-phaser-game)
* [Phaser官方API](http://phaser.io/docs)
* [Phaser新手教程](http://bbs.9ria.com/forum.php?mod=viewthread&tid=404929)
* [Phaser游戏开发教程](http://www.ipastimes.com/category/Phaser/)
* [开始一个Phaser教程](https://stonetingxin.gitbooks.io/phaser/content/introudce/setupaproject.html)
* [详解Phaser游戏框架状态管理state](http://www.iphaser.cn/learn/stateManager)
* [用Phaser来制作一个HTML5游戏](http://www.cnblogs.com/2050/p/3790279.html)
* [Creature - Advance 2D Skeletal + Mesh Animation Tool now with Phasor Support](http://www.html5gamedevs.com/topic/13290-creature-advance-2d-skeletal-mesh-animation-tool-now-with-phasor-support/)
* [Phaser Runtimes](http://www.kestrelmoon.com/creaturedocs/Game_Engine_Runtimes_And_Integration/Phaser_Runtimes%20.html)
* [TypeScript官方](http://www.typescriptlang.org/)
* [TypeScript中文手册](https://zhongsp.gitbooks.io/typescript-handbook/content/)


    

