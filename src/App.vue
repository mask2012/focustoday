<template>
  <div id="app">
    <div class="background_holder">
      <span class="bg_shadow fadein"></span>
      <span class="bg_img fadein" :style="'background:url('+bgImg+') center center/cover no-repeat'"></span>
    </div>

    <div class="region region_center">
      <div class="time">{{currentTime}}</div>
      <div class="greeting">Good morning, Mask.</div>
    </div>

    <div class="region region_center_below">
      <div class="todo_list_holder">
        <div class="todo_list">
          <div class="todo_input">
            <h3>What is your main focus for today?</h3>
            <input
              type="text"
              autocomplete="off"
              maxlength="35"
              v-model="newTodo"
              @keyup.enter="addTodo"
            />
          </div>
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
import Unsplash, { toJson } from "unsplash-js";
import fetch from "node-fetch";
global.fetch = fetch;

const unsplash = new Unsplash({
  accessKey: "Hr-gc8bEf0AnAC0XRo1WUNvP9oCw4A9ddAOdhYzJ674",
  secret: "44WWENgCIrtD-H-y1xTPK90IaN_vaTeihYxpgoJBD_0",
  headers: {
    "X-Custom-Header": "mask2012"
  },
  timeout: 5000 // values set in ms
});
// unsplash.users.profile("naoufal")
//   .catch(err => {
//     // Your flawless error handling code
//     console.log('unsplash error goes here',err);
//   });

// unsplash.search.photos("dogs", 1, 10, { orientation: "portrait" })
//   .then(toJson)
//   .then(json => {
//     console.log('json',json);
//     // Your code
//   });

export default {
  name: "App",
  components: {
    // HelloWorld
  },
  data() {
    return {
      todos: todoStorage.fetch(), //todo全集
      newTodo: "", //新的todo
      listHeight: document.documentElement.clientHeight * 0.2,
      liHeight: 0,
      liFontSize: 10,
      currentTime: moment().format("HH:mm"),
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
          this.liFontSize > 40 ? 40 + this.liFontSize / 10 : this.liFontSize;
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
    },

    removeTodo: function(todo) {
      console.log(todo);
      this.todos.splice(this.todos.indexOf(todo), 1);
    },
    toggleTodo: function(todo) {
      const currentIndex = this.todos.indexOf(todo);
      this.todos[currentIndex].completed = !this.todos[currentIndex].completed;
    }
  },
  mounted() {
    this.liHeight = this.listHeight / this.todos.length;
    this.liFontSize = (this.listHeight * 0.8) / this.todos.length;

    window.onresize = () => {
      this.listHeight = document.documentElement.clientHeight * 0.2;
      this.liHeight = this.listHeight / this.todos.length;
      this.liFontSize = (this.listHeight * 0.8) / this.todos.length;
      this.liFontSize =
        this.liFontSize > 40 ? 40 + this.liFontSize / 10 : this.liFontSize;
    };

    setInterval(() => {
      this.currentTime = moment().format("HH:mm");
    }, 700);

    unsplash.photos
      .getRandomPhoto({ username: "naoufal" })
      .then(toJson)
      .then(json => {
        console.log("json", json);
        this.bgImg=json.urls.regular
      });
  }
};
</script>

<style lang='less'>
@import "assets/css/global.css"; /*引入公共样式*/
@import "App.less";
</style>
