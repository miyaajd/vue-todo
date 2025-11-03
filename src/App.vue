<template>
  <div class="wrap">
    <h1>To Do List</h1>
    <button @click="toggleDarkMode">
      {{ isDark ? 'Light Mode' : 'Dark Mode' }}
    </button>
    <Todo :is-dark="isDark" />
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'
import Todo from './components/Todo.vue'
const isDark = ref(false)

// 로드 시 로컬 스토리지에서 모드 불러오기
onMounted(() => {
  const saveMode = localStorage.getItem('isDark')
  if (saveMode !== null) {
    isDark.value = saveMode === 'true'
    updateMode()
  }
})

// 다크모드 토글
const toggleDarkMode = () => {
  isDark.value = !isDark.value
  updateMode()
  // 로컬 스토리지에 저장
  localStorage.setItem('isDark', isDark.value)
}

const updateMode = () => {
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
  // 브라우저 상단 theme-color 변경
  const metaThemeColor = document.querySelector('meta[name=theme-color]')
  if (metaThemeColor) {
    metaThemeColor.setAttribute('content', isDark.value ? '#1e1e1e' : '#fafdff')
  }
}

// isDark 값 바뀔 때마다 ToDo 컴포넌트에 반영되도록 감시
watch(isDark, (newVal)=>{
  localStorage.setItem('isDark', newVal)
})
</script>

<style scoped>
.wrap {
  width: 100%;
  height: 100vh;
  max-height: 100%;
  background-color: #fafdff;
  display: flex;
  flex-direction: column;
  align-items: center;
}
h1 {
  width: 100%;
  text-align: center;
  padding: 50px 0 0;
  color: #555;
  font-weight: 500;
  font-size: 24px;
}
/* 토글버튼 스타일 */
button {
  margin: 20px 0;
  padding: 8px 16px;
  border-radius: 50px;
  border: none;
  background-color: #1e1e1e;
  color: #fefefe;
  font-size: 12px;
  cursor: pointer;
}
/* 다크모드 스타일 */
.dark .wrap {
  background-color: #1e1e1e;
}
.dark h1 {
  color: #fefefe;
}
.dark button {
  background-color: #fefefe;
  color: #1e1e1e;
}
@media screen and (max-width: 768px) {
  h1 {
    font-size: 24px;
    padding: 30px 0 0;
  }
}
</style>
