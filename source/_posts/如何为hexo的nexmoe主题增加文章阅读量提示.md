---
title: 如何为hexo的nexmoe主题增加文章阅读量提示
date: 2021-03-24 13:42:52
tags: [前端, 博客建设]
categories: 前端
cover: https://gitee.com/Zarathos/blog-image-library/raw/master/d670231da37d8a1cde12cce5342682a546a66ed4.jpg@1320w_742h.webp
coverWidth: 1319
coverHeight: 742
---
昨天我通过github+hexo搭建了这个个人博客,一开始使用的是@[TriDiamond](https://github.com/TriDiamond)大佬的[obsidian](https://github.com/TriDiamond/hexo-theme-obsidian)
主题,不过在使用了一天之后，发现这个主题模板不大适合我的个人审美...(但主题本身非常优秀，科技感十足，建议喜欢科技风的各位去看看)于是我今天早上将博客主题换成了@[折影轻梦](https://github.com/nexmoe)大佬的[nexmoe](https://github.com/theme-nexmoe/hexo-theme-nexmoe)主题
<br/>但是在obsidian主题中，有一个可以显示文章被点击阅读的次数的功能，而在nexmoe中却没有这个功能，由于我对该功能比较喜欢，所以我决定手动把这个功能添加进去。
<!--more-->
***
![obsidian主题展示](example1.png)
![nexmoe主题展示](example2.png)
***
以下是添加步骤:
***
<font color="red">注：本文使用的统计插件为不蒜子(busuanzi)，这是它的网址→[不蒜子](http://busuanzi.ibruce.info/)
</font><br/>
<b>第一步</b>
<br/>打开/node_modules/hexo-theme-nexmoe/layout/_partial/_post/meta.ejs
<br/>在开头添加如下代码
```css
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"> </script>
```
随后在&lt;/div&gt;标签的上面，添加如下代码:
```css
<a><span id="busuanzi_container_page_pv">
         本文总阅读量<span id="busuanzi_value_page_pv"></span>次
       </span></a>
```
保存，在终端中输入`hexo s`并在本地进行浏览查看效果
![](example3.png)
成功添加了阅读量提示功能。
***
<b>第二步,为统计功能增加一个开关</b>
有时候，我们可能忽然改变主意，想要将这个统计功能关闭，其实这也很简单,我们只需要把刚才添加的代码改一下
```html
   <% if (theme.busuanzi&&theme.busuanzi.enable) { %> <a><span id="busuanzi_container_page_pv">
         本文总阅读量<span id="busuanzi_value_page_pv"></span>次
       </span></a>
        <% } %>
```
全部代码如图所示
![全部代码](code1.png)
之后打开<b>Hexo根目录</b>下的_config.nexmoe.yml文件
"[关于为什么是在hexo根目录下而不是主题根目录下可见此](https://docs.nexmoe.com/hexo-nexmoe/zhu-ti-pei-zhi)"
添加如下代码
```
busuanzi:
 enable: true
```
即可,我们可以将其修改为`enable: false`来随时关闭这个统计功能了!