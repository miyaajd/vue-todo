<template>
  <div class="toDo">
    <div class="input">
      <input
        type="text"
        v-model="newTodo"
        @keyup.enter="addTodo"
        placeholder="할 일을 입력하세요"
      />
      <button class="add" @click="addTodo">+</button>
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <span :class="{ done: todo.done }" @click="toggleDone(index)"
          >•&nbsp;&nbsp;&nbsp;&nbsp; {{ todo.text }}</span
        >
        <button class="delete" @click="removeTodo(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'
const newTodo = ref('')
const todos = ref([])

watch(
  todos,
  (val) => {
    localStorage.setItem("todos", JSON.stringify(val))
  },
  { deep: true }
)

onMounted(() => {
  const saved = localStorage.getItem("todos")
  if (saved) {
    todos.value = JSON.parse(saved)
  }
})

const addTodo = () => {
  if (!newTodo.value.trim()) return
  todos.value.push({ text: newTodo.value, done: false })
  newTodo.value = ''
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const toggleDone = (index) => {
  todos.value[index].done = !todos.value[index].done
}
</script>

<style scoped>
.toDo {
  width: 100%;
  max-width: 500px;
  height: 600px;
  max-height: 100000000000px;
  border-radius: 16px;
  background-color: #fafafa;
  box-shadow: 0 0 100px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 30px;
  box-sizing: border-box;
}
.done {
  text-decoration: line-through;
  color: #ccc;
}
button {
  padding: 5px 8px;
  border: none;
  background-color: transparent;
  margin-left: 16px;
  cursor: pointer;
}
.add {
  font-size: 18px;
  border: 1px solid #999;
  border-radius: 50%;
  padding: 5px 10px;
  color: #999;
}
.delete {
  color: dodgerblue;
  margin-left: 16px;
}
input {
  padding: 8px 24px;
  outline: none;
  border-radius: 5px;
  border: 1px solid #999;
}
ul {
  list-style: none;
  margin-top: 30px;
}
li {
  margin: 10px 0;
  text-wrap: wrap;
  word-break: break-word;
}
span {
  cursor: pointer;
  font-size: 18px;
  color: #333;
}
@media screen and (max-width: 768px) {
  .toDo {
    width: 320px;
  }
  span {
    font-size: 14px;
  }
  .delete {
    font-size: 12px;
  }
}
</style>
