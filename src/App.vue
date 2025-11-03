<script setup>
import { ref } from 'vue';
import Todo from './components/Todo.vue'
const isDark  = ref(false)
const toggleDarkMode  = ()=>{
  isDark.value = !isDark.value
  if(isDark.value){
    document.documentElement.classList.add("dark")
  }else{
    document.documentElement.classList.remove("dark")
  }
}
</script>

<template>
  <div class="wrap">
    <h1>To Do List</h1>
    <button @click="toggleDarkMode">
      {{  isDark ? "Light Mode" : "Dark Mode" }}
    </button> 
    <Todo :is-dark="isDark"/>
  </div>
</template>
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
button{
  margin:20px 0;
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
.dark h1{
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
