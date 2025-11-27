<!-- src/App.vue -->
<template>
  <div class="app-container">
    <div class="background"></div>
    <Home />
    <footer>
      <span>© 2025 Made in <a href="https://mlhh.cn" target="_blank">十安 | </a></span>
      <a href="https://cloud.tencent.com/act/pro/eo-freeplan?ad_trace=3fe11a4b31634407819b427b61a2b768&from=28455&from_column=28455" target="_blank">腾讯云边缘安全加速EdgeOne</a>(国内)提供强劲加速 | 
      <a href="https://beian.miit.gov.cn/" target="_blank">豫ICP备2024077903号-3</a>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Home from './components/Home.vue';

// 环境变量引用
const userName = ref(import.meta.env.VITE_APP_USER_NAME);
const icpNumber = ref(import.meta.env.VITE_APP_ICP_NUMBER);
const policenumber = ref(import.meta.env.VITE_APP_POLICE_NUMBER);
</script>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  /* 可以在这里添加背景样式 */
}

footer {
  text-align: center;
  padding: 20px;
  font-size: 0.9rem;
  color: var(--text-color);
  margin-top: auto;
  width: 100%;
  box-sizing: border-box;
}

footer a {
  color: var(--text-color);
  text-decoration: none;
  transition: color 0.3s ease;
}

footer a:hover {
  text-decoration: underline;
  color: var(--link-hover-color, #42b983); /* 使用Vue默认的链接 hover 颜色作为备选 */
}

/* 移动端适配 */
@media screen and (max-width: 768px) {
  footer {
    font-size: 0.8rem;
    padding: 15px 10px;
    line-height: 1.6;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  
  /* 移除垂直排列时的分隔线 */
  footer span + a::before,
  footer a + a::before {
    content: none;
  }
}
</style>
