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

<style lang='less'>
  @import 'assets/css/global.css'; /*引入公共样式*/
  @import 'App.less';
</style>
