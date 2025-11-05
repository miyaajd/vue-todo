<template>
  <div class="wrap">
    <h1>To Do List</h1>
    <button @click="toggleDarkMode">
      {{ isDark ? 'Light Mode' : 'Dark Mode' }}
    </button>
    <Todo :is-dark="isDark" />
    <p class="howto" @click="showModal">How to use</p>
    <div class="modal" v-if="modal">
      <div class="modal-box">
        <h3 class="modal-title">How to use</h3>
        <p class="modal-txt">
          • 다크모드 / 라이트모드 버튼으로 전환가능 (상태저장됨) <br />
          • 할 일 입력 후 플러스 버튼으로 리스트에 추가 <br />
          • 완료한 리스트는 텍스트를 클릭해 비활성화 가능 <br />
          • <strong>Delete</strong> 클릭 시 리스트 행 삭제 가능 <br />
          • <strong>Kitty</strong> 클릭 시 리스트 내용 수정 가능 <br />
          • 리스트 행 최대 갯수는 <strong>12개</strong>, 더 필요하면 개발자에게로<br />
        </p>
        <button class="close" @click="modal = false">× Close</button>
      </div>
    </div>
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
watch(isDark, (newVal) => {
  localStorage.setItem('isDark', newVal)
})

// showModal
const modal = ref(false)
const showModal = () => {
  modal.value = !modal.value
}
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
.howto {
  color: #7e7e7e;
  text-decoration: underline;
  font-size: 14px;
  margin-top: 10px;
  cursor: pointer;
}
/* 모달 */
.modal {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal-box {
  position: absolute;
  top: 50%;
  left: 50%;
  min-width: 90%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 50px 20px 40px 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 30px;
  border-radius: 16px;
}
.modal-title {
  font-weight: 500;
  color: #1e1e1e;
}
.modal-txt {
  color: #1e1e1e;
  font-size: 14px;
  line-height: 2;
}
.close {
  background-color: transparent;
  color: #1e1e1e;
  border: 1px solid #1e1e1e;
  margin: 0;
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
.dark .howto {
  color: #9e9e9e;
}
.dark .modal {
  background-color: rgba(0, 0, 0, 0.8);
}
.dark .modal-box {
  background-color: #1e1e1e;
}
.dark .modal-title{
  color: #fefefe;
}
.dark .modal-txt {
  color: #fefefe;
}
.dark .close  {
  color: #fafafa;
  background-color: #1e1e1e;
  border-color: #fafafa;
}
@media screen and (max-width: 768px) {
  h1 {
    font-size: 24px;
    padding: 30px 0 0;
  }
}
</style>
