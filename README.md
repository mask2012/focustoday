# focusToday 
a chrome crx main focus on todo list everyday
发现chrome插件momentum挺好用的，不过todoList多写一条都要收费，一年200，还挺贵，咱自己写一个还不行吗，说干就干，我先整个基础版的，但起码可以无拘无束写todo list呀


## 需求分析
**基本需求**
1. 初次进入引导，询问名字
2. 时间展示
3. 问候语
4. 背景图一日一换

**高级需求**
1. 天气播报
2. todoList
3. 格言一日一换
4. 常用工具 ★★


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












