<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link :to="{ name :'About' }">About</router-link>
    <router-link :to="{ name :'Jobs' }">Jobs</router-link>
</div>
   <button @click="redirect">Redirect</button> 
   <button @click="back">Go Back</button>
   <button @click="forward">Go Forward</button>
  <router-view/>
</template>
<script>
export default {
  methods: {
    redirect() {
      this.$router.push({name: 'Home'})
    },
    back() {
      this.$router.go(-1)
    },
    forward() {
      this.$router.go(1)
    }
  }
}</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
  text-decoration: none;
  padding: 10px;
  border-radius: 4px;
}

nav a.router-link-exact-active {
  color: white;
  background-color: crimson;
}
button{
  margin: 0 10px;
  padding: 10px;
  border: none;
  border-radius: 4px;
}
</style>

<script setup>
import {computed, ref} from "vue";

  let todoList = ref([]);
  const newTodoName = ref('');
  const todoId = ref(0);

  const readLocalStorage = () => {
    const data = localStorage.getItem('todoList');
    if (data) {
      todoList.value = JSON.parse(data);
      todoId.value = todoList.value.length;
    }
  };

  const writeLocalStorage = () => {
    localStorage.setItem('todoList', JSON.stringify(todoList.value));
  };

  const addTodo = () => {
    todoList.value.push({
      id: todoId.value++,
      name: newTodoName.value || 'New Todo',
      done: false
    });
    newTodoName.value = '';
    writeLocalStorage();
  };

  const removeTodo = (id) => {
    todoList.value.splice(todoList.value.findIndex(todo => todo.id === id), 1);
    writeLocalStorage();
  };


  const toggleTodo = (id) => {
    todoList.value = todoList.value.map(todo => {
      if (todo.id === id) {
        todo.done = !todo.done;
      }
      return todo;
    });
    writeLocalStorage()
  };

  const completedTodoNum = computed(() => {
    return todoList.value.filter(todo => todo.done).length;
  });

  readLocalStorage();

</script>

<template>
  <top>
      <up></up>
      <header>
          <h1>To Do List &#127800;</h1>
      </header>
      <main>
          <div id="todoInput">
              <div class="listItem">
                  <input type="text" v-model="newTodoName" placeholder="Tambahkan to do &#128151;" @keydown.enter="addTodo" @keydown.esc="removeTodo(todoList[0].id)">
              </div>
          </div>
          <div id="todoList" v-for="todo in todoList" :key="todo.id">
          <input type="checkbox" id="done" name="done" value="done" @click="toggleTodo(todo.id)">
              <div class="listItem">
                  <div v-if="todo.done" class="todoNameDone" @click="toggleTodo(todo.id)">{{todo.name}}</div>
                  <div v-else class="todoName" @click="toggleTodo(todo.id)">{{todo.name}}</div>
                  <button @click="removeTodo(todo.id)">X</button>
              </div>
          </div>
      </main>
  </top>

</template>
