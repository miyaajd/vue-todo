<template>
  <div :class="['toDo', { dark: props.isDark }]">
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
        <div class="list">
          <img src="/public/icons/kitty.png" alt="kitty" class="kitty" @click="editTodo(index)" />
          <span :class="{ done: todo.done }" @click="toggleDone(index)">{{ todo.text }}</span>
        </div>
        <button class="delete" @click="removeTodo(index)">Del</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, defineProps } from 'vue'
const newTodo = ref('')
const todos = ref([])

// props로 다크모드 제어
const props = defineProps({
  isDark: Boolean,
})

// todos 저장 (localStorage sync)
watch(
  todos,
  (val) => {
    localStorage.setItem('todos', JSON.stringify(val))
  },
  { deep: true },
)

// 앱 로드 시 저장된 투두 불러오기
onMounted(() => {
  const saved = localStorage.getItem('todos')
  if (saved) {
    todos.value = JSON.parse(saved)
  }
})

// 투두추가
const addTodo = () => {
  if (todos.value.length >= 12) {
    alert('Maximum reached. Please remove some items.')
    return
  }
  if (!newTodo.value.trim()) return
  todos.value.push({ text: newTodo.value, done: false })
  newTodo.value = ''
}

// 투두삭제
const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

// 완료 상태 토글 (텍스트 비활성화 토글기능)
const toggleDone = (index) => {
  todos.value[index].done = !todos.value[index].done
}

// 추가된 투두 내용 수정
const editTodo = (index) => {
  const current = todos.value[index].text
  const updated = prompt('Edit To-Do', current)
  if (updated !== null && updated.trim() !== '') {
    todos.value[index].text = updated.trim()
  }
}
</script>

<style scoped>
.toDo {
  width: 100%;
  max-width: 500px;
  height: 550px;
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
  cursor: pointer;
  display: inline-block;
  text-wrap: nowrap;
}
.add {
  font-size: 18px;
  color: #999;
}
.delete {
  color: dodgerblue;
  margin-left: 0;
}
.input {
  display: flex;
  width: 100%;
  justify-content: space-between;
}
input {
  padding: 8px 24px;
  outline: none;
  border-radius: 5px;
  border: 1px solid #999;
  width: 100%;
}
ul {
  width: 95%;
  margin: auto;
  list-style: none;
  margin-top: 10px;
}
li {
  margin: 10px 0;
  width: 100%;
  text-wrap: wrap;
  word-break: break-word;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.list {
  display: flex;
  align-items: center;
}
span {
  margin-left: 16px;
  cursor: pointer;
  font-size: 18px;
  color: #555;
}
/* 다크모드 스타일 */
.toDo.dark {
  background-color: #3e3e3e;
}
.toDo.dark span {
  color: #fefefe;
}
.toDo.dark .done {
  color: #999;
}
/* 모바일 스타일 */
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
.kitty {
  width: 20px;
  display: inline-block;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  cursor: pointer;
}
</style>
