<template>
  <div :class="titleClass">
    <h1>{{ message.split('').reverse().join('') }}</h1>
    <button @click="increment">click</button>
    <p>count is : {{ counter.count }}</p>
    <p>cnt is : {{ cnt }}</p>
    <input v-model="text">
    <p>{{ text }}</p>

    <button @click="toggle">toggle</button>
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
  </div>

  <div>
    <form @submit.prevent="addTodo">
      <input v-model="newTodo">
      <button>Add Todo</button>    
    </form>
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done">
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)">X</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
  </div>
  
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

// component logic
// declare some reactive state here.
// reactive() only works on ★objects★ (including arrays and built-in types like Map and Set
const counter = reactive({
    count: 0
}) 
let id = 0

const cnt = ref(0)
const message = ref('Hello world')
const titleClass = ref('title') 
const text = ref('')
const awesome = ref(true)
const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])
const hideCompleted = ref(false)
const filteredTodos = computed(() => {
  return hideCompleted.value // todos.value.done의 값이 
    ? todos.value.filter(item => !item.done)
    : todos.value
})


function increment() {
  counter.count++
  cnt.value++
}
function toggle() {
  awesome.value = !awesome.value
}
function addTodo() {
  todos.value.push({
    id: id++, 
    text: newTodo.value,
    done: false,
  })
  newTodo.value = ''
}
function removeTodo(todo) {
  // const target_id = todo.id
  //  The filter method does not modify the original array; 
  // it returns a new array with the filtered elements. 
  todos.value = todos.value.filter(item => item != todo)

}
</script>

<style>
li {
  list-style-type: none;
}
#app {
  font-family: Fira code Light, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.title {
  color: brown;
}
.done {
  text-decoration: line-through;
}
</style>
