<template>
  <div id="app">
    <div class="background_holder">
      <span class="bg_shadow fadein"></span>
      <span class="bg_img fadein"></span>
      <!-- <span class="bg_img fadein" :style="'background:url('+bgImg+') center center/cover no-repeat'"></span> -->
    </div>

    <div class="region region_center">
      <div class="time">{{currentTime}}</div>
      <div class="greeting">{{greetingTxt}}, {{yourName}}.</div>
    </div>

    <div class="region region_center_below">
      <div class="todo_list">
        <transition name="inputTrans" mode="out-in">
          <div class="todo_input" v-if="isShowInput" key="keyInput">
            <h3>What is your main focus for today?</h3>
            <input
              type="text"
              autocomplete="off"
              maxlength="35"
              v-model="newTodo"
              @keyup.enter="addTodo"
            />
          </div>
          <div class="todo_add" @click="toggleInput()" v-else key="keyAdd"></div>
        </transition>

        <div class="todo_scroll" v-show="todos.length" v-cloak>
          <ul :style="'height:'+listHeight+'px'">
            <li
              v-for="todo in todos"
              :key="todo.id"
              :class="{ li_done: todo.completed}"
              :style="'height:'+liHeight+'px; line-height:'+liHeight+'px; font-size:'+liFontSize+'px'"
              @click="toggleTodo(todo)"
            >
              <span>{{ todo.title }}</span>
              <i class="ico_destroy" @click="removeTodo(todo)"></i>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";

var STORAGE_KEY = "focustoday";
var todoStorage = {
  fetch: function() {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    todos.forEach(function(todo, index) {
      todo.id = index;
    });
    todoStorage.uid = todos.length;
    return todos;
  },
  save: function(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  }
};

import moment from "moment";

// import Unsplash, { toJson } from "unsplash-js";
// import fetch from "node-fetch";
// global.fetch = fetch;

// const unsplash = new Unsplash({
//   accessKey: "Hr-gc8bEf0AnAC0XRo1WUNvP9oCw4A9ddAOdhYzJ674",
//   secret: "44WWENgCIrtD-H-y1xTPK90IaN_vaTeihYxpgoJBD_0",
//   headers: {
//     "X-Custom-Header": "mask2012"
//   },
//   timeout: 5000 // values set in ms
// });

export default {
  name: "App",
  components: {
    // HelloWorld
  },
  data() {
    return {
      todos: todoStorage.fetch(), //todo全集
      newTodo: "", //新的todo

      listHeight: 0, //todo list整体高度
      liHeight: 0, //todo list li 高度
      liFontSize: 10, //li里边字号
      currentTime: "", //时间
      isShowInput: true, //是否展示input。展示input和展示add是互斥的
      greetingTxt: "", //欢迎文字
      yourName: "Mask", //用户姓名
      bgImg: ""
    };
  },
  watch: {
    todos: {
      handler: function(todos) {
        todoStorage.save(todos);
        this.liHeight = this.listHeight / todos.length;
        this.liFontSize = (this.listHeight * 0.8) / this.todos.length;
        this.liFontSize =
          this.liFontSize > 30 ? 30 + this.liFontSize / 14 : this.liFontSize;
      },
      deep: true
    }
  },
  methods: {
    addTodo: function() {
      var value = this.newTodo && this.newTodo.trim();
      if (!value) {
        return;
      }
      this.todos.push({
        id: todoStorage.uid++,
        title: value,
        completed: false
      });
      this.newTodo = "";

      //隐藏input
      this.toggleInput();
    },

    removeTodo: function(todo) {
      console.log(todo);
      this.todos.splice(this.todos.indexOf(todo), 1);
      //todo清为0后，展示input
      if(this.todos.length==0){
        this.isShowInput=true
      }
    },
    toggleTodo: function(todo) {
      const currentIndex = this.todos.indexOf(todo);
      if (currentIndex == -1) {
        return;
      }
      this.todos[currentIndex].completed = !this.todos[currentIndex].completed;
    },
    calculateHeight() {
      this.listHeight = document.documentElement.clientHeight * 0.25;
      this.liHeight = this.listHeight / this.todos.length;
      this.liFontSize = (this.listHeight * 0.8) / this.todos.length;
      this.liFontSize =
        this.liFontSize > 30 ? 30 + this.liFontSize / 14 : this.liFontSize;
    },
    toggleInput() {
      this.isShowInput = !this.isShowInput;
    },
    initClockAndGreeting() {
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
      setInterval(() => {
        this.currentTime = moment().format("HH:mm");
        this.greetingTxt = moment().format("a");
      }, 700);
    }
  },
  mounted() {
    //设置高度
    this.calculateHeight();

    //窗口变化时重新设置高度
    window.onresize = () => {
      this.calculateHeight();
    };

    //初始化clock和greeting
    this.initClockAndGreeting();

    //
    this.isShowInput=this.todos.length==0
  }
};
</script>

<style lang='less'>
@import "assets/css/global.css"; /*引入公共样式*/
@import "App.less";
</style>
