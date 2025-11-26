<template>
  <div class="background"></div>
  <Home />
  <footer>
    <span>© 2025 Made in <a href="https://mlhh.cn" target="_blank">十安</a></span> | 
    <a href="https://cloud.tencent.com/act/pro/eo-freeplan?ad_trace=3fe11a4b31634407819b427b61a2b768&from=28455&from_column=28455" target="_blank">腾讯云边缘安全加速EdgeOne</a>(国内)提供强劲加速 | 
    <a href="https://beian.miit.gov.cn/" target="_blank">豫ICP备2024077903号-3</a>
  </footer>
</template>

<script setup>
import { ref } from 'vue';
import Home from './components/Home.vue';

const userName = ref(import.meta.env.VITE_APP_USER_NAME);
const icpNumber = ref(import.meta.env.VITE_APP_ICP_NUMBER);
const policenumber = ref(import.meta.env.VITE_APP_POLICE_NUMBER);
</script>
