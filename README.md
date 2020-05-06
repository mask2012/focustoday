# focusToday 
a chrome crx main focus on todo list everyday  
发现chrome插件momentum挺好用的，不过todoList多写一条都要收费，一年200，还挺贵  
咱自己写一个还不行吗，说干就干，我先整个基础版的，但起码可以无拘无束写todo list呀


## 需求分析  
**基本需求**  
1. 初次进入引导，询问名字
2. 时间展示 ✓
3. 问候语 ✓ui
4. 背景图一日一换 ✓

**高级需求**  
1. todoList  ✓
2. 格言一日一换
3. 常用工具 ★★
4. 天气播报
5. 白屏页


## 开发过程记录  
**天气接口**  
https://www.sojson.com/blog/305.html

**背景图-一日一换，访问即可**  
```css
background: url(//area.sinaapp.com/bingImg) center center no-repeat; 
background-size: cover;
```

顺便找到了bing一日一壁纸的列表   
 http://bing.plmeizi.com/

**谷歌应用商店chrome扩展程序和APP的发布流程**   
https://chromecj.com/utilities/2018-05/1406.html

**360官方的插件开发文档**  
http://open.chrome.360.cn/extension_dev/overview.html

**vue项目传到github小坑**  
vue项目关联github仓库还挺坑，没法用git小乌龟完成，步骤如下
1. 新建vue项目时勾选“初始化git仓库”（默认是勾选的），这样就不需要自己去git bash here然后git init了
2. 项目内右键打开git bash here关联到github仓库 ```git remote add origin https://github.com/mask2012/focustoday.git```
3. git push -u origin master推到github
注意：github仓库创建时不要有readme也不要提交，第一次就要来源于vue项目，否则git会认为是两个项目，不给push

**第一版只做了ui，下一步搞定时间，欢迎，和todolist吧**  
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/20200429181257.png)



**晚上把todo list做好了**  
参照了[vue官方的todo demo](https://www.cnblogs.com/jyyzzjl/p/6252279.html)，砍掉了很多功能，我只需要基础的  
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/20200430091937.png)


**突发奇想我可以把todo 列表做成如果条数多就字大，条数少就字小**    
尝试了直接用display:flex发现并不会无限制缩小行高，而且在结合overflow:scroll的时候，顶部会出现断头，这可能就是flex造成的bug了，所以这条路断了  
另一条路就是在resize window的时候实时获取todo列表的高度，再结合todo列表条数，得到每条高度和字号，这样就可以条数越多字越小了，而且保证不会出现滚动条
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/1.gif)


**以后也可以弄一个番茄工作计时，到时提醒的功能**  
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/20200430162255.png)


**修饰一下滚动条的style**  
https://www.cnblogs.com/vicky-li/p/css.html




**设计logo**
花了半个小时就搞定了logo，不得不说，中文logo制作的都是一坨屎  
google搜logo generator出来的结果是真心牛逼，不过竟然是收费的，好在小图我也勉强够用了  
https://editor.freelogodesign.org/?lang=en&companyname=focus&category=0  
https://www.brandcrowd.com/maker/logo/43b1190b-1b7c-4842-be79-443a9265c73e?text=focustoday  


用在线ps简单处理了一下  
https://ps.gaoding.com/#/

最终logo长成这样  
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/focus_logo.png)

用在线facicon生成一下即可
https://www.favicon-generator.org/


**发现壁纸需要的是landscape类的图片，发现momentum使用的是unsplash提供的服务**    
官方说不限流量，只需注册开发者即可，看起来很不错的样子
https://unsplash.com/developers  

又看了一会，发现他的api是不收费的  
demo版的限制是一小时50次；高级版一小时5k次，你只需要按照他的要求使用，比如必须注明图片源于unsplash，写明作者，等等等等
https://help.unsplash.com/en/articles/3887917-when-should-i-apply-for-a-higher-rate-limit

接下来是unsplash的使用方法
https://github.com/unsplash/unsplash-js

调用api后发现下载还是挺慢的，也没法直接满足我一日换一张的需求  
使用unsplash的api能实现的就是2种思路  
一种是search关键字，一种是手动加入collection，然后请求时带上page，实现这个page就会比较麻烦  

由此发现了好多图片站  
国外的  
https://www.liaosam.com/free-hd-photo-stocks.html
国内的  
https://www.zhihu.com/question/265481379

甚至还发现了用爬虫的方法
https://blog.csdn.net/qq_41536331/article/details/81028749  

目前我想到的最好的方法应该是
1. 爬虫爬取照片
2. 照片上传github，用jsdelivr加速访问
3. 启动后台服务，每日更新图片地址，前端直接请求此服务，得到图片url
4. 为了保证图片不会占满github仓库，只需保留7张图片，定时删除无用的图片
5. 保证爬取，上传，更新配置，删除无用图片流程不间断自动执行

