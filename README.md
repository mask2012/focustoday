# focusToday 
a chrome crx main focus on todo list everyday  
发现chrome插件momentum挺好用的，不过todoList多写一条都要收费，一年200，还挺贵  
咱自己写一个还不行吗，说干就干，我先整个基础版的，但起码可以无拘无束写todo list
 

## 需求分析   
**基本需求**  
1. 初次进入引导，询问名字
2. 时间展示 ✓
3. 问候语 ✓
4. 背景图一日一换 ✓half

**高级需求**  
1. todoList  ✓
2. 格言一日一换
3. 常用工具 ★★
4. 天气播报
5. 白屏页


## 开发过程记录 
在开发的过程中接触到很多收费的服务，如天气，图片，logo  

**天气接口**  
https://www.sojson.com/blog/305.html  
Momentum用的是AccuWeather  
[彩云天气](http://wiki.swarma.net/index.php/%E5%BD%A9%E4%BA%91%E5%A4%A9%E6%B0%94API/v2) - 支持全球天气数据，两种空气质量数据，天气预报，实况天气，独家降水预报，独家空气质量预报，六种天气数据，四种生活指数数据等内容，部分功能收费。
[和风天气](http://www.kancloud.cn/hefengyun/weather/222344) - 支持7-10天预报，实况天气，每小时预报，生活指数，灾害预警，景点天气，历史天气，城市查询等内容，仅国内数据免费。
[心知天气](http://www.thinkpage.cn/doc) - 支持天气实况，逐日预报和历史，24小时逐小时预报，过去24小时天气历史记录，气象灾害预警，空气质量实况与城市排行，逐日和逐小时空气质量预报，过去24小时空气质量历史记录，生活指数，农历、节气、生肖，机动车尾号限行，日出日落，月初月落和月像，城市搜索等内容，仅国内数据免费。


**背景图-一日一换，访问即可**  
最简单的做法就是直接使用bing的图片，不过质量不算太好，先凑合用
```css
background: url(//area.sinaapp.com/bingImg) center center no-repeat; 
background-size: cover;
```

顺便找到了bing一日一壁纸的列表   
 http://bing.plmeizi.com/

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
一种是search关键字，一种是手动加入collection，请求时都需要带上page，实现这个page就会比较麻烦   
在实际的使用中发现api特别慢，图片下载也特别慢，最后api直接就挂了，
想访问unsplash也是打不开了，悲剧。  
在中国做任何正常的事情都很不容易，直接使用unsplash估计是没戏了

由此也发现了好多图片站    
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


**处理成插件**   

360官方的插件开发文档  
http://open.chrome.360.cn/extension_dev/overview.html

这个cliper插件实战教程很不错  
https://github.com/ecmadao/Coding-Guide/blob/master/Notes/%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B/Chrome%E6%89%A9%E5%B1%95%E7%A8%8B%E5%BA%8F%E5%BC%80%E5%8F%91.md

这一篇也写了很多心得  
https://www.jianshu.com/p/9c4a552cc984

**打开新tab即打开本地页面**  
做成插件，基本就是增加一个manifest.json文件，然后加入一些配置，然后再弄一些文件  
在这篇文章里终于找到了overrides这个参数来覆盖新tab达到我想要的效果  
https://www.jianshu.com/p/9c4a552cc984  

**最终成型的manifest.json是这样的**  
```json
{
    "name": "focusToday",
    "version": "1.0",
    "manifest_version": 2,
    "description": "focus today is a todo list with backgrounds when new tab",
    "author": "mask",
    "icons": {
        "16": "icon_16.png",
        "128": "icon_128.png"
    },
    "permissions": [
    ],
    "chrome_url_overrides": {
        "newtab": "index.html"
    }
}
```

**安装插件**  
现在已经不可以直接拖拽crx进行安装了  
那么不要钱的让对方使用你的插件的方法是：
1. 打包成zip传给对方
2. 对方解压此zip
3. 管理扩展程序-->开发者模式-->加载已解压的扩展程序
4. 找到解压目录，即可导入

**文件结构**  
在文件结构上，本来想把manifest.json放在根目录，无奈导入的时候提示我node_modules里有报错  
那就只能放在dist目录里了，缺点是每次build后需要手动copy一些文件进去  

**vue项目修改页面title**  
算一个小坑吧，修改package.json是可行的，但不能有空格，要求很严格  
另一个方法是新增vue.config.js文件，内容如下  
只需要把必要的填进去就可以了，我只是为了改个页面title而已
```js
module.exports = {
    pages: {
      index: {
        // entry for the page
        entry: 'src/main.js',
        // the source template
        template: 'public/index.html',
        // output as dist/index.html
        // filename: 'index.html',
        // when using title option,
        // template title tag needs to be <title><%= htmlWebpackPlugin.options.title %></title>
        title: 'New tab',
        // chunks to include on this page, by default includes
        // extracted common chunks and vendor chunks.
        // chunks: ['chunk-vendors', 'chunk-common', 'index']
      },
      // when using the entry-only string format,
      // template is inferred to be `public/subpage.html`
      // and falls back to `public/index.html` if not found.
      // Output filename is inferred to be `subpage.html`.
    //   subpage: 'src/subpage/main.js'
    }
  }
  ```

**国内时间和国外时间**  
这个挺有意思，做的时候才了解得这么细  
这是国内的  
```js
moment.updateLocale("zh-cn", {
  meridiem: function(hour, minute) {
    if (hour < 9) {
      return "早上";
    } else if (hour < 11 && minute < 30) {
      return "上午";
    } else if (hour < 13 && minute < 30) {
      return "中午";
    } else if (hour < 18) {
      return "下午";
    } else {
      return "晚上";
    }
  }
});
this.greetingTxt = moment().format("a");
```

这是国外的  
```js
moment.updateLocale("en", {
  meridiem: function(hour) {
    if (hour < 12) {
      return "good morning";  //从子夜持续至中午或从太阳升起时持续到中午
    }else if (hour >= 12 && hour < 18) {
      //When it comes to greeting someone, Noon is used in the same context under "afternoon". You never hear anyone saying "Good Noon", it's always "Good Afternoon" when PM strikes.  Noon is midday or 12:00pm sharp. Anything beyond 12:00 pm until roughly around 5:00-5:30pm is afternoon.
      return "good afternoon";
    } else if (hour >= 18 && hour < 20) {
      return "good evening"; //从下午到夜晚日光逐渐减弱的时期
    } else {
      return "good night";  //指晚上8点之后，一般是夜间看不到太阳的时间段
    }
  }
});
this.greetingTxt = moment().format("a");
```

**切换输入和增加按钮**  
参考 https://cn.vuejs.org/v2/guide/transitions.html  
发现vue的transition用来做些小动画还真的是方便啊
关键就是记住这张图  
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/20200508145556.png)  
特别是在有一组元素来回切的时候，制作不要太方便

做出来的效果是这样的  
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/focus_gif2.gif)  

觉得不太好，换成fade感觉更好一点  
![](https://cdn.jsdelivr.net/gh/mask2012/imgBed/focus_gif3.gif)  
这里边有2个细节  
1. 出场持续时间短，先慢后快，入场持续时间长，先快后慢，符合肉眼看到的事物规律
2. 注意css优先级的坑，加了transition的元素的class名前边就不要再有限制了，否则下边的这一段会压不过原有的，就不会有动画效果

```css
/* 进场  https://cubic-bezier.com/#.43,.08,.6,.93 */
.inputTrans-enter-active {
  transition: all 0.5s cubic-bezier(.43,.08,.6,.93);
}
/* 出场 */
.inputTrans-leave-active {
  transition: all 0.3s cubic-bezier(.11,.42,.76,.78);
}
/* 端点 */
.inputTrans-enter, .inputTrans-leave-to{
  opacity:0;
}
```




