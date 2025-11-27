<template>
  <div class="app-container">
    <div class="background"></div>
    <Home />
    <footer>
      <span>© 2025 Made in <a href="https://mlhh.cn" target="_blank" rel="noopener">十安 | </a></span>
      <a href="https://cloud.tencent.com/act/pro/eo-freeplan?ad_trace=3fe11a4b31634407819b427b61a2b768&from=28455&from_column=28455" target="_blank" rel="noopener">腾讯云边缘安全加速EdgeOne</a>(国内)提供强劲加速 | 
      <a href="https://beian.miit.gov.cn/" target="_blank" rel="noopener">{{ icpNumber }}</a>
    </footer>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import Home from './components/Home.vue';

// 优化：使用computed缓存环境变量
const icpNumber = computed(() => import.meta.env.VITE_APP_ICP_NUMBER || '豫ICP备2024077903号-3');
</script>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.background {
  position: fixed;
  inset: 0;
  z-index: -1;
  background-size: cover;
  background-position: center;
}

footer {
  text-align: center;
  padding: 1rem;
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
  color: var(--link-hover-color, #42b983);
}

@media screen and (max-width: 768px) {
  footer {
    font-size: 0.8rem;
    padding: 0.8rem 0.5rem;
    line-height: 1.6;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
  
  footer span + a::before,
  footer a + a::before {
    content: none;
  }
}
</style>
