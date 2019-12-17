---
title: Hexo + git线上版本管理建立个人博客
date: 2019-12-13 10:23:28
description: 使用版本管理及开源Hexo创建免费个人博客流程
tags: 
    - Hexo
    - 博客
categories: 
    - [学习,博客]
    - [参考文件]
---
## 搭建个人博客
参考文章：
码云 + Hexo 搭建个人博客：https://blog.csdn.net/weidong_y/article/details/90904781

### 使用Gitee/Github线上版本管理内置的pages创建仓库
Gitee生成仓库pages：注意`使用用户名同名仓库才可以用 name.gitee.io 一级域名访问`<br/>
    参考-https://blog.csdn.net/qq_36667170/article/details/79318578<br/>
Github生成仓库Pages：注意`只能是 name.github.io格式仓库`<br/>
    参考-https://pages.github.com/<br/>

### 使用Hexo创建博客

#### 安装npm(Nodejs)
>[ubuntu安装最新版node和npm](https://www.jianshu.com/p/f2592d106aac)<br/>
[Windows安装使用npm（Nodejs）](https://blog.csdn.net/han0373/article/details/80606487)

#### 开始配置Hexo

##### Hexo相关命令
初始化：空文件夹下执行>>>hexo init
创建新文章页：根目录中>>>hexo new "文章名字"
创建功能文件：根目录中>>>hexo new page categories/tags (创建Hexo功能文件)
清空静态页面：根目录中>>>hexo clean (部署不生效可以先clean下)
生成静态页面：根目录中>>>hexo g
启动本地服务：根目录中>>>hexo s
提交部署服务：根目录中>>>hexo deploy

##### 下载主题
* 通过https://hexo.io/themes/找到想要的主题git路径
* 在项目根目录下的themes文件夹中
* 执行git clone [主题git路径]https://github.com/theme-next/hexo-theme-next.git

##### 配置主题
修改项目根目录下的_config.yml中theme为下载的主题文件夹名字

##### 创建文章
方法一
* hexo new "文章名字"

方法二
* 直接创建md格式文件复制到/source/_posts目录下，文件头加入下面代码
 `冒号后面一定要跟一个空格`<br/>
```
    ---
    title: a
    date: 2019-04-14 23:10:17
    ---
```

##### 主题风格
主题文件下的_config.yml`注意：不是根目录是主题目录：/themes/主题文件夹名称/_config.yml`
找到Schemes模块，修改为scheme: Gemini

##### 博客左侧栏设置
参考文件:[hexo文件参数及其相关说明](https://www.jianshu.com/p/d1dedae4d970)<br/>
/_config.yml文件配置
```
title: 个人博客（网站标题）
subtitle: hineqi.github.io（名字下显示的小标题）
description: 记录下自己的动态（描述内容）
keywords: （搜录百度google后的搜索关键词）
author: 齐海阳（作者）
language: zh-Hans（网站使用的语言,默认是英语(主题languages文件夹下)）
timezone: ''（网站时区，Hexo 默认使用电脑的时区）
```

##### 分类设置
参考：[Hexo 一篇文章多个 categories](https://www.jianshu.com/p/bff1b1845ac9)
* 在项目根目录下，执行下面的命令行，新建分类页面，然后会在项目根目录下的 source 文件夹中新建一个 categories 文件夹。
```
hexo new page categories
```
* 打开 categories 文件夹中的 index.md 文件，添加 type 字段，设置为 “categories”。
* 接着到主题文件夹下的 _config.yml 配置文件下，找到 menu 模块，把 categories 的注释给去掉。<br/>
`tags同理 注意：所有文章头部都要添加categories及内容就可以自动分类，zh-Hans.yml语言下需要把menu 模块空格去掉`
```
    ---
    title: Hexo + git线上版本管理建立个人博客
    date: 2019-12-13 10:23:28
    tags: [Hexo,博客]
    categories: [学习,参考,帮助文档]
    或
    tags: 
        - Hexo
        - 博客
    categories: 
        - [学习,博客]   （学习.博客这种子集目录）
        - [参考]       
    ---
```
##### 博客优化
###### Hexo博客添加站内搜索
要看主题支持什么搜索，NexT主题支持Swiftype/微搜索/Local Search/Algolia
下面介绍Local Search`安装在根目录`
- 安装`hexo-generator-search`
```
npm install hexo-generator-search --save
```
- 安装`hexo-generator-searchdb`
```
npm install hexo-generator-searchdb --save
```
- 根目录下的`_config.yml`配置文件末尾添加如下代码
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
- 编辑主题文件下`_config.yml`配置文件，设置`Local search`模块下`enable`为`true`
###### 博客头像设置
- 主题文件下`_config.yml`配置文件，`Sidebar Avatar`模块下`avatar`的`url`设置为图像链接
或者主题文件下/source/images中添加图片文件，然后`url`内容为`/images/图片名称.png`
###### 设置头像圆角并旋转打开
- 打开主题文件夹的`source\css\_common\components\sidebar`目录下的`sidebar-author.styl`文件，然后把下面的代码添加进去即可。
```css
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;
  /* 头像圆形 */
  border-radius: 80px;
  -webkit-border-radius: 80px;
  -moz-border-radius: 80px;
  box-shadow: inset 0 -1px 0 #333sf;
  /* 设置循环动画 [animation: (play)动画名称 (2s)动画播放时长单位秒或微秒 (ase-out)动画播放的速度曲线为以低速结束 
    (1s)等待1秒然后开始动画 (1)动画播放次数(infinite为循环播放) ]*/
 
  /* 鼠标经过头像旋转360度 */
  -webkit-transition: -webkit-transform 1.0s ease-out;
  -moz-transition: -moz-transform 1.0s ease-out;
  transition: transform 1.0s ease-out;
}
img:hover {
  /* 鼠标经过停止头像旋转 
  -webkit-animation-play-state:paused;
  animation-play-state:paused;*/
  /* 鼠标经过头像旋转360度 */
  -webkit-transform: rotateZ(360deg);
  -moz-transform: rotateZ(360deg);
  transform: rotateZ(360deg);
}
/* Z 轴旋转动画 */
@-webkit-keyframes play {
  0% {
    -webkit-transform: rotateZ(0deg);
  }
  100% {
    -webkit-transform: rotateZ(-360deg);
  }
}
@-moz-keyframes play {
  0% {
    -moz-transform: rotateZ(0deg);
  }
  100% {
    -moz-transform: rotateZ(-360deg);
  }
}
@keyframes play {
  0% {
    transform: rotateZ(0deg);
  }
  100% {
    transform: rotateZ(-360deg);
  }
}
```
###### 右上角fork me设置
- 在[GitHub Corners](http://tholman.com/github-corners/)上选择喜欢的挂饰，复制代码
- 打开主题目录下layout文件夹下_layout.swig,`<div class="headband"></div>标签后`粘贴上面代码，href改为自己github链接
###### 动态背景
- 打开主题目录下layout文件夹的_layout.swig,文末加上下面代码
```
<!-- 动态背景 -->
<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
```
###### 背景图片设置
- 打开主题目录下source/css/_custom/custom.styl文件
```css
body{
    background:url(/images/bg.jpg);
    background-size:cover;
    background-repeat:no-repeat;
    background-attachment:fixed;
    background-position:center;
    // 设置主题部分的透明度，具体看图
    opacity: 0.8;
}
```
###### 点击出现桃心效果设置
- 主题文件目录/source/js/src新建clicklove.js,将下面代码copy进去
```javascript
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);
```
- 然后主题目录下/layout/_layout.swig末尾加上下面代码
```javascript
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/clicklove.js"></script>
```
###### 首页文章预览设置
