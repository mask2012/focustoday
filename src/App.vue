<template>
  <div id="app">
    <div class="background_holder">
      <span class="bg_shadow fadein"></span>
      <span class="bg_img fadein"></span>
    </div>

    <div class="region region_center">
      <div class="time">16:27</div>
      <div class="greeting">晚上好, Mask</div>
    </div>

    <div class="region region_center_below">
      <div class="todo_list_holder">
        <div class="todo_list">
          <h3>今天的待做事项</h3>
          <input type="text" data-test="focus-input" autocomplete="off" max-length="7" />
          <ul>
            <li class="li_done">修顺风电脑</li>
            <li>找到袜子</li>
            <li>修顺风电脑</li>
            <li>找到袜子</li>
            <li>修顺风电脑</li>
            <li class="li_done">修顺风电脑</li>
            <li>找到袜子</li>
            <li>修顺风电脑</li>
            <li>找到袜子</li>
            <li>修顺风电脑</li>
            <li>找到袜子</li>
          </ul>
        </div>
      </div>
    </div>

    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    // HelloWorld
  }
};
</script>

<style lang="less">
/*reset*/
html,body,div,span,applet,object,iframe,h1,h2,h3,h4,h5,h6,p,blockquote,pre,a,abbr,acronym,address,big,cite,code,del,dfn,em,img,ins,kbd,q,s,samp,small,strike,strong,sub,sup,tt,var,b,u,i,center,dl,dt,dd,ol,ul,li,fieldset,form,label,legend,table,caption,tbody,tfoot,thead,tr,th,td,article,aside,canvas,details,figcaption,figure,footer,header,hgroup,menu,nav,section,summary,time,mark,audio,video{margin:0;padding:0;border:0;font-size:100%;font:inherit;vertical-align:baseline}article,aside,details,figcaption,figure,footer,header,hgroup,menu,nav,section{display:block}img,svg{vertical-align:top}ol,ul{list-style:none}blockquote,q{quotes:none}blockquote:before,blockquote:after,q:before,q:after{content:'';content:none}table{border-collapse:collapse;border-spacing:0}a[href],label[for],select,input[type=checkbox],input[type=radio]{cursor:pointer}button,input[type=button],input[type=image],input[type=reset],input[type=submit]{padding:0;overflow:visible;cursor:pointer}button::-moz-focus-inner,input[type=button]::-moz-focus-inner,input[type=image]::-moz-focus-inner,input[type=reset]::-moz-focus-inner,input[type=submit]::-moz-focus-inner{border:0}.hide{position:absolute!important;left:-9999em!important}.clearfix:after{content:".";display:block;visibility:hidden;clear:both;height:0}h1,h2,h3,h4,h5,h6{font-weight:normal}.ie7 .clearfix{zoom:1}strong{font-weight:bold}em{font-style:italic}del{text-decoration:line-through}th,td{vertical-align:top}th{font-weight:normal;text-align:left}address,cite,dfn{font-style:normal}abbr,acronym{border-bottom:1px dotted #999;cursor:help}sub{line-height:0}sup{top:-0.5em}sub{bottom:-0.25em}textarea{overflow:auto}img{width: 100%; height: auto;}

body {
  text-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
  cursor:default
}
body,
html {
  font-size: 100%;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  font-family: -apple-system, BlinkMacSystemFont, "Neue Haas Grotesk Text Pro",
    "Helvetica Neue", Helvetica, Arial, sans-serif;
}

.fadein {
  animation: fadein 0.5s ease-out 0s forwards;
}
@keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/*背景*/
.background_holder {
  display: block;
}
.background_holder .bg_shadow {
  position: fixed;
  z-index: 2;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: url(assets/overlay-vignette.png);
  background-size: 100% 100%;
}
.background_holder .bg_img {
  position: absolute;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: url(//area.sinaapp.com/bingImg) center center no-repeat;
  background-size: cover;
}

/*钟*/
.region {
  position: absolute;
  z-index: 11;
  width: 100%;
}

.region_center {
  left: 0;
  top: 42%;
  transform: translate(0, -50%);
  text-align: center;
}
.region_center_below {
  left: 0;
  top: 61%;
  text-align: center;
}
.time {
  font-size: 800%;
}
.greeting {
  font-size: 300%;
}

/*todoList*/
.todo_list_holder{ display: flex; justify-content: center;}
.todo_list h3{font-weight: normal; font-size: 180%;margin: 0 0 5px}
.todo_list input {
    width: 100%;
    padding-top: 4px;
    display: block;
    background: 0;
    border: 0;
    border-bottom: 2px solid #fff;
    color: #fff;
    font-weight: 500;
    outline: 0;
    text-align: center;
    transition: border-color .2s ease;
    font-size: 137%;
    line-height: 1.7;
    margin-bottom: 30px;
}
.todo_list ul{
  height: 190px;
  overflow-y: scroll;
  font-size: 100%;
}
.todo_list ul li:before{
  content:'';
  display: inline-block;
  margin-right: 4px;
  vertical-align: middle;
  position: relative; top: -2px;
  width: 17px;
  height: 17px;
  background: url(data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iaWNvbiIgdmlld0JveD0iMCAwIDEwMjQgMTAyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iMjAwIiBoZWlnaHQ9IjIwMCI+PHBhdGggZD0iTTUxMiA5NjBDMjY0Ljk2IDk2MCA2NCA3NTkuMDQgNjQgNTEyUzI2NC45NiA2NCA1MTIgNjRzNDQ4IDIwMC45NiA0NDggNDQ4LTIwMC45NiA0NDgtNDQ4IDQ0OHptMC04MzJjLTIxMS43NDQgMC0zODQgMTcyLjI1Ni0zODQgMzg0czE3Mi4yNTYgMzg0IDM4NCAzODQgMzg0LTE3Mi4yNTYgMzg0LTM4NC0xNzIuMjU2LTM4NC0zODQtMzg0eiIgZmlsbD0iI2ZmZiIvPjwvc3ZnPg==);
  background-size: 100% auto;
}
.todo_list ul li{
  margin-bottom: 5px;
}
.todo_list ul li:hover{
  color: #eee;
}
.todo_list ul li.li_done{
  text-decoration: line-through;
  color: #eee;
}
.todo_list ul li.li_done:before{
  background: url(data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iaWNvbiIgdmlld0JveD0iMCAwIDEwMjQgMTAyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iMjAwIiBoZWlnaHQ9IjIwMCI+PHBhdGggZD0iTTUxMiA5NjBDMjY0Ljk2IDk2MCA2NCA3NTkuMDQgNjQgNTEyUzI2NC45NiA2NCA1MTIgNjRzNDQ4IDIwMC45NiA0NDggNDQ4LTIwMC45NiA0NDgtNDQ4IDQ0OHptMC04MzEuNzEyYy0yMTEuNTg0IDAtMzgzLjcxMiAxNzIuMTI4LTM4My43MTIgMzgzLjcxMiAwIDIxMS41NTIgMTcyLjEyOCAzODMuNzEyIDM4My43MTIgMzgzLjcxMiAyMTEuNTUyIDAgMzgzLjcxMi0xNzIuMTYgMzgzLjcxMi0zODMuNzEyUzcyMy41NTIgMTI4LjI4OCA1MTIgMTI4LjI4OHptMjE0Ljk3NiAyNjQuODk2YTMxLjk2OCAzMS45NjggMCAwIDAtNDUuMjQ4LjI1NmwtMjMzLjI4IDIzNS44NC0xMDMuMjY0LTEwNi4xMTJhMzIgMzIgMCAxIDAtNDUuODg4IDQ0LjYwOEw0MjUuMzEyIDY5Ny4yOGMuMDY0LjA5Ni4xOTIuMDk2LjI1Ni4xOTJsLjE2LjI1NmMyLjAxNiAxLjk4NCA0LjUxMiAzLjIgNi44OCA0LjU0NCAxLjI0OC42NzIgMi4yNCAxLjc5MiAzLjUyIDIuMzA0YTMxLjc0NCAzMS43NDQgMCAwIDAgMjQuMDY0LjA2NGMxLjI0OC0uNTEyIDIuMjA4LTEuNTM2IDMuMzkyLTIuMTc2IDIuNC0xLjM0NCA0Ljg5Ni0yLjUyOCA2Ljk0NC00LjU0NC4wNjQtLjA2NC4wOTYtLjE5Mi4xOTItLjI1Ni4wNjQtLjA5Ni4xNi0uMTI4LjI1Ni0uMTkyTDcyNy4yIDQzOC40NjRhMzIuMDMyIDMyLjAzMiAwIDAgMC0uMjI0LTQ1LjI4eiIgZmlsbD0iI2ZmZiIvPjwvc3ZnPg==);
  background-size: 100% auto;
}



</style>
