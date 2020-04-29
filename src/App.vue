<template>
  <div id="app">
    <div class="background_holder">
      <span class="bg_shadow fadein"></span>
      <span class="bg_img fadein"></span>
    </div>

    <div class="region_clock">
      <div class="time">16:27</div>
      <div class="greeting">Good morning, Mask</div>
    </div>

    <div class="region_todo_list"></div>

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
body { margin: 0; padding: 0; text-shadow: 0 1px 5px rgba(0,0,0,.1);}
body,html{font-size: 100%;}
#app { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; color: #fff; font-family: -apple-system, BlinkMacSystemFont, "Neue Haas Grotesk Text Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;;}

.fadein{animation:fadein 0.5s ease-out 0s forwards}
@keyframes fadein{
  from{opacity:0}to{opacity:1}
}

/*背景*/
.background_holder{ display: block;}
.background_holder .bg_shadow{ position: fixed; z-index: 2; left: 0; top: 0; width: 100%; height: 100%; background: url(assets/overlay-vignette.png); background-size:100% 100%;}
.background_holder .bg_img{ position: absolute; z-index: 1; left: 0; top: 0; width: 100%; height: 100%; background: url(//area.sinaapp.com/bingImg) center center no-repeat; background-size: cover;}

/*钟*/
.region_clock{ position: absolute; z-index: 11; left: 0; top: 40%; width: 100%; transform: translate(0,-50%); text-align: center;}
.time{  font-size: 800%;}
.greeting{  font-size: 300%;}


/*todoList*/




</style>
