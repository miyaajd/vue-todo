<script setup>
import ToDo from './components/ToDo.vue';

</script>

<template>
  <div class="wrap">
    <h1>◎ To Do List ◉</h1>
    <ToDo/>
  </div>
</template>

<style scoped>
.wrap{
  width: 100%;
  height: 100vh;
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
@media screen and (max-width:768px) {
  h1{
    font-size: 24px;
    padding: 30px 0;
  }
}
</style>
