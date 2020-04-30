<template>
  <div id="app">
    <div class="background_holder">
      <span class="bg_shadow fadein"></span>
      <span class="bg_img fadein"></span>
    </div>

    <div class="region region_center">
      <div class="time">16:27</div>
      <div class="greeting">Good morning, Mask.</div>
    </div>

    <div class="region region_center_below">
      <div class="todo_list_holder">
        <div class="todo_list">
          <h3>What is your main focus for today?</h3>
          <input
            type="text"
            autocomplete="off"
            maxlength="30"
            v-model="newTodo"
            @keyup.enter="addTodo"
          />
          <div class="todo_scroll">
            <ul v-show="todos.length" v-cloak>
              <li v-for="todo in todos" :key="todo.id" :class="{ li_done: todo.completed}">
                <span @click="toggleTodo(todo)">{{ todo.title }}</span>
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


export default {
  name: "App",
  components: {
    // HelloWorld
  },
  data() {
    return {
      todos: todoStorage.fetch(),
      newTodo: "",
      editedTodo: null,
      visibility: "all"
    };
  },
  // watch todos change for localStorage persistence
  watch: {
    todos: {
      handler: function(todos) {
        todoStorage.save(todos);
      },
      deep: true
    }
  },
  // methods that implement data logic.
  // note there's no DOM manipulation here at all.
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
      const currentIndex=this.todos.indexOf(todo)
      this.todos[currentIndex].completed=!this.todos[currentIndex].completed
    },


  },
  mounted() {
    
  }
};
</script>

<style lang="less">
/*reset*/
html,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
img,
svg {
  vertical-align: top;
}
ol,
ul {
  list-style: none;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: "";
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
a[href],
label[for],
select,
input[type="checkbox"],
input[type="radio"] {
  cursor: pointer;
}
button,
input[type="button"],
input[type="image"],
input[type="reset"],
input[type="submit"] {
  padding: 0;
  overflow: visible;
  cursor: pointer;
}
button::-moz-focus-inner,
input[type="button"]::-moz-focus-inner,
input[type="image"]::-moz-focus-inner,
input[type="reset"]::-moz-focus-inner,
input[type="submit"]::-moz-focus-inner {
  border: 0;
}
.hide {
  position: absolute !important;
  left: -9999em !important;
}
.clearfix:after {
  content: ".";
  display: block;
  visibility: hidden;
  clear: both;
  height: 0;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: normal;
}
.ie7 .clearfix {
  zoom: 1;
}
strong {
  font-weight: bold;
}
em {
  font-style: italic;
}
del {
  text-decoration: line-through;
}
th,
td {
  vertical-align: top;
}
th {
  font-weight: normal;
  text-align: left;
}
address,
cite,
dfn {
  font-style: normal;
}
abbr,
acronym {
  border-bottom: 1px dotted #999;
  cursor: help;
}
sub {
  line-height: 0;
}
sup {
  top: -0.5em;
}
sub {
  bottom: -0.25em;
}
textarea {
  overflow: auto;
}
img {
  width: 100%;
  height: auto;
}

body {
  text-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
  cursor: default;
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
  background: url(assets/bg1.jpg);
  // background: #555 url(//area.sinaapp.com/bingImg) center center no-repeat;
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
  top: 40%;
  transform: translate(0, -50%);
  text-align: center;
}
.region_center_below {
  left: 0;
  top: 58%;
  text-align: center;
}
.time {
  font-size: 1040%;
}
.greeting {
  font-size: 330%;
}

/*todoList*/
.todo_list_holder {
  display: flex;
  justify-content: center;
}
.todo_list h3 {
  font-weight: normal;
  font-size: 194%;
  margin: 0 0 5px;
}
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
  transition: border-color 0.2s ease;
  font-size: 180%;
  line-height: 1.5;
  margin-bottom: 26px;
}

.todo_scroll{ background: rgba(0,0,0,.2); border-radius: 6px; padding: 15px 10px;}
.todo_list ul {
  height: 190px;
  overflow-y: scroll;
  font-size: 110%;
}
.todo_list ul li:before {
  content: "";
  display: inline-block;
  margin-right: 8px;
  vertical-align: middle;
  position: relative;
  top: -1px;
  width: 20px;
  height: 20px;
  background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNNSAyQzMuMzQ2IDIgMiAzLjM0NiAyIDV2MTRjMCAxLjY1NCAxLjM0NiAzIDMgM2gxNGMxLjY1NCAwIDMtMS4zNDYgMy0zVjVjMC0xLjY1NC0xLjM0Ni0zLTMtM0g1em0xOSAzdjE0YTUgNSAwIDAgMS01IDVINWE1IDUgMCAwIDEtNS01VjVhNSA1IDAgMCAxIDUtNWgxNGE1IDUgMCAwIDEgNSA1eiIgZmlsbD0iI2ZmZiIvPjwvc3ZnPg==);
  background-size: 100% auto;
}
.todo_list ul li {
  margin-bottom: 14px;
  line-height: 1.1;
  cursor: pointer;
}
.todo_list ul li:hover {
  color: #eee;
}
.todo_list ul li.li_done {
  text-decoration: line-through;
  color: #eee;
}
.todo_list ul li.li_done:before {
  background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNMTAuMDQxIDE3bC00LjUtNC4zMTkgMS4zOTUtMS40MzUgMy4wOCAyLjkzN0wxNy4wMzcgN2wxLjQyMiAxLjQwOUwxMC4wNDEgMTd6TTUgMkMzLjM0NiAyIDIgMy4zNDYgMiA1djE0YzAgMS42NTQgMS4zNDYgMyAzIDNoMTRjMS42NTQgMCAzLTEuMzQ2IDMtM1Y1YzAtMS42NTQtMS4zNDYtMy0zLTNINXptMTkgM3YxNGE1IDUgMCAwIDEtNSA1SDVhNSA1IDAgMCAxLTUtNVY1YTUgNSAwIDAgMSA1LTVoMTRhNSA1IDAgMCAxIDUgNXoiIGZpbGw9IiNmZmYiLz48L3N2Zz4=);
  background-size: 100% auto;
}
.todo_list ul li.li_done .ico_destroy{ opacity: 1; pointer-events: auto;}
.ico_destroy{ display: inline-block; opacity: 0; pointer-events: none; vertical-align: middle; margin-left: 15px; position: relative; top: -2px; width: 20px; height: 20px; background: url(data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iaWNvbiIgdmlld0JveD0iMCAwIDEwNDUgMTAyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iMjA0LjEwMiIgaGVpZ2h0PSIyMDAiPjxwYXRoIGQ9Ik0yODIuNTE3IDIxMy4zNzZsLTQ1LjM1NCA0NS4xNjNMNDg5LjQ3MiA1MTJsLTI1Mi4zMSAyNTMuNDYxIDQ1LjM1NSA0NS4xNjMgMjUyLjA5Ni0yNTMuMjcgMjUyLjA5NiAyNTMuMjcgNDUuMzU1LTQ1LjE2My0yNTIuMjg4LTI1My40NEw4MzIuMDY0IDI1OC41NGwtNDUuMzU1LTQ1LjE2My0yNTIuMDk2IDI1My4yNDgtMjUyLjA5Ni0yNTMuMjI3eiIgZmlsbD0iI2ZmZiIvPjwvc3ZnPg==); background-size: 100% auto;}
</style>
