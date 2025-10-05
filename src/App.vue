<script setup>
import Todo from './components/Todo.vue'
</script>

<template>
  <div class="wrap">
    <h1>◎ To Do List ◉</h1>
    <Todo />
  </div>
</template>
<style scoped>
.wrap {
  width: 100%;
  height: 100vh;
  max-height: 100%;
  background-color: #333;
  display: flex;
  flex-direction: column;
  align-items: center;
}
h1 {
  width: 100%;
  text-align: center;
  padding: 50px 0;
  color: #fff;
  font-weight: 500;
}
@media screen and (max-width: 768px) {
  h1 {
    font-size: 24px;
    padding: 30px 0;
  }
}
</style>
