<template>
  <div>
    <h1 id="title">{{ message }}</h1>
    <button @click="increment">
      Count<span class="numbers" v-if="startCount">: {{ counter.count }}</span>
    </button>
    <button @click="startText">Show</button>
    <p v-if="showText">The starting count is: {{ counter.starting_count }}</p>
    <br />
    <input v-model="newTodo" placeholder="Type here" /> <button @click="addTodo">Add</button>

    <ul>
      <li
        v-for="todo in todos"
        :key="todo.id"
        @click="togglePressed(todo)"
        :class="{ hidden: hideCompleted && todo.pressed }"
      >
        <div class="todo-item">
          <p :class="{ done: todo.pressed }">{{ todo.text }}</p>
          <button @click.stop="removeTodo(todo)">X</button>
        </div>
      </li>
    </ul>
  </div>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide elements' }}
  </button>
</template>

<script setup>
import { ref, onMounted, reactive } from 'vue'

// reactive state
var id = 0
const newTodo = ref('')
const todos = ref([])
const message = ref('Hello, World!') // component logic
const showText = ref(false)
const startCount = ref(false)
const counter = reactive({
  // declare some reactive state here
  count: 0,
  starting_count: 0,
  n_count: 0
})
const hideCompleted = ref(false)

function togglePressed(todo) {
  todo.pressed = !todo.pressed
}
function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo) {
  const index = todos.value.indexOf(todo)
  if (index !== -1) {
    todos.value.splice(index, 1)
  }
}

// functions that mutate state and trigger updates
function increment() {
  counter.count += 10
  counter.n_count++
  startCount.value = true
}

function startText() {
  showText.value = !showText.value
  if (showText.value) {
    setTimeout(() => {
      showText.value = false
    }, 3000)
  }
}

// lifecycle hooks
onMounted(() => {
  console.log(`The initial count is ${counter.starting_count}`)
})
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
}
.todo-item button {
  margin-left: 10px;
  font-size: 12px;
}
.done {
  text-decoration: line-through;
}

.hidden {
  display: none;
}

.numbers {
  color: green;
}

#title {
  color: blue;
  font-size: 34px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  margin-top: 20px;
}

p {
  color: red;
  font-size: 18px;
  margin-top: 20px;
}
</style>
