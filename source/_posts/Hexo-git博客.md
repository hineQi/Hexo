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
top: 1
copyright: true
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
创建新草稿页：根目录中>>>hexo new draft "文章名字"
发布草稿文章：根目录下>>>hexo publish "文章名字"
创建功能文件：根目录中>>>hexo new page categories/tags (创建Hexo功能文件)
清空静态页面：根目录中>>>hexo clean (部署不生效可以先clean下)
生成静态页面：根目录中>>>hexo g
启动本地服务：根目录中>>>hexo s
预览草稿服务：根目录中>>>hexo s -draft
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
##### 文章引用图片
前提：修改根目录下_config.yml中的`post_asset_folder: true`，new 新建的文章就会自动新建一个同名文件夹可以放资源
- 安装Hexo图片引用插件
```
npm install https://github.com/CodeFalling/hexo-asset-image --save
```
- 文章中引用图片
```
![图片信息](文章名/图片名.jpg)
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
- 修改主题目录下_config.yml下的canvas_nest为true
- 或打开主题目录下layout文件夹的_layout.swig,文末加上下面代码
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
- `<!--more-->`手动切断
- 添加`description`
    在文章的 front-matter 中添加 description 和 photos 字段
- 自动形成摘要，在主题目录下_config.yml添加
```
auto_excerpt:
  enable: true
  length: 150
```
###### 修改文章内链接文本样式
修改主题目录下/source/css/_common/components/post/post.styl文末添加
```
// 文章内链接文本样式
.post-body p a{
  color: #0593d3; //原始链接颜色
  border-bottom: none;
  border-bottom: 1px solid #0593d3; //底部分割线颜色
  &:hover {
    color: #fc6423; //鼠标经过颜色
    border-bottom: none;
    border-bottom: 1px solid #fc6423; //底部分割线颜色
  }
}
```
###### 修改文章底部标签样式
修改主题目录下/layout/_macro/post.swig中，搜索`rel="tag">#`，将`#`替换成`<i class="fa fa-tag"></i>`
###### 在文章末尾添加“文章结束”标记
- 主题目录下/layout/_macro文件夹中新建passage-end-tag.swig文件
- passage-end-tag.swig中添加以下内容
```
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">-------------本文结束<i class="fa fa-paw"></i>感谢您的阅读-------------</div>
    {% endif %}
</div>
```
- 修改主题目录下/layout/_macro/post.swig，在`post-body`之后，`post-footer`之前
```
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```
- 修改主题目录下/_config.yml，末尾添加：
```
# 文章末尾添加“本文结束”标记
passage_end_tag:
  enabled: true
```
###### 主页文章添加阴影效果
主题目录下/source/css/_custom/custom.styl，添加以下代码
```
// 主页文章添加阴影效果
 .post {
   margin-top: 60px;
   margin-bottom: 60px;
   padding: 25px;
   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
  }
```
###### 添加网页顶部进度加载条
主题目录下_config.yml，搜索`pace`
###### RSS设置
- 在根目录安装hexo插件
```
npm install --save hexo-generator-feed
```
- 安装后在根目录_config.yml末尾加下面代码
```
# Extensions
## Plugins: http://hexo.io/plugins/
plugins: hexo-generate-feed
```
- 在主题目录下_config.yml中找到rss，在后面加上 /atom.xml
###### 社交小图标设置
- 在主题目录下_config.yml中搜索Social
- 图标可以在[Font Awesome Icon](https://fontawesome.com/icons?from=io)查找
###### 友情链接设置
- 在主题目录下_config.yml中搜索links_title
###### 去掉底部Hexo相关信息
在主题目录下/layout/_partials/footer.swig注释相关代码
###### 添加底部桃心
- 直接修改主题目录下_config.yml中的footer下icon
- 或在主题目录下/layout/_partials/footer.swig，搜索with-love，在[fontawesom](https://fontawesome.com/icons?d=gallery&q=heart)上找到喜欢的图标
###### 设置网站Favicon
- 在[阿里巴巴矢量图库](https://www.iconfont.cn/?spm=a313x.7781069.1998910419.d4d0a486a)中找图片替换主题目录下/source/images里三张图
`apple-touch-icon-next.png`,`favicon-16x16-next.png`,`favicon-32x32-next.png`
- 或修改主题目录下_config.yml的favicon
###### 博客置顶设置
- 安装插件
```
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```
- 在置顶文章头部添加`top`,数值越大越靠前
```
---
title: this is my first blog
date: 2019-04-14
top: 100
---
```
- 主题目录下/layout/_macro/post.swig，定位到post-header
```
{% if post.top %}
  <i class="fa fa-thumb-tack"></i>
  <font color=7D26CD>置顶</font>
{% endif %}
```
###### 在文章底部增加版权信息
- 在主题目录下/layout/_macro/下添加文件`my-copyright.swig`
```
{% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>
  
  <!-- JS库 sweetalert 可修改路径 -->
  <script src="https://cdn.bootcss.com/jquery/2.0.0/jquery.min.js"></script>
  <script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>
  <p><span>本文标题:</span><a href="{{ url_for(page.path) }}">{{ page.title }}</a></p>
  <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>最后更新:</span>{{ page.updated.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank" title="Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)">署名-非商业性使用-禁止演绎 4.0 国际</a> 转载请保留原文链接及作者。</p>  
</div>
<script> 
    var clipboard = new Clipboard('.fa-clipboard');
    $(".fa-clipboard").click(function(){
      clipboard.on('success', function(){
        swal({   
          title: "",   
          text: '复制成功',
          icon: "success", 
          showConfirmButton: true
          });
    });
    });  
</script>
{% endif %}
```
- 在主题目录下/source/css/_common/components/post下添加文件`my-post-copyright.styl`
```
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #b5b5b5;
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #a3d2a3;
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}
```
- 修改主题目录下/layout/_macro/post.swig，`wechat_subscriber`上面
```
<div>
      {% if not is_index %}
        {% include 'my-copyright.swig' %}
      {% endif %}
</div>
```
- 在主题文件/source/css/_common/components/post/post.styl末尾添加
```
@import "my-post-copyright"
```
- 在文章头部添加`copyright`
```
---
title: Hexo-NexT主题配置
date: 2018-01-20 20:41:08
categories: Hexo
tags:
- Hexo
- NexT
top: 100
copyright: true
---
```
###### 文章代码主题设置
主题目录下_config.yml，highlight_theme

## 部署个人博客
- 在代码线上管理复制仓库地址
- 项目根目录_config.yml，修改deploy值
```
deploy:
  type: git
  repo: git@github.com:hineQi/hineqi.github.io.git
  branch: master
```
- 项目根目录，打开Git Bash
```
git config --global user.name空格+你的码云的名字
git config --global user.email空格+你的码云的邮箱
```
- 安装`hexo-deployer-git`
```
npm install hexo-deployer-git --save
```