<template>
  <div class="content">
    <div class="user-profile-container">
      <div class="avatar-container">
        <img :src="avatarUrl" alt="Avatar" class="avatar" />
      </div>
      <div class="user-info">
        <h1>{{ userName || '访客' }}</h1>
        <div class="typed-container">
          <span id="typed-output"></span>
          <span class="cursor">|</span>
        </div>
      </div>
    </div>
    
    <Website />
    <VisitTimer />
    
    <div class="action-buttons">
      <button @click="toggleDarkMode" :title="darkModeConfig.tooltipText">
        <i :class="darkModeConfig.iconClass" :style="{ color: darkModeConfig.hoverColor }"></i>
      </button>
      <button @click="toggleAbout">
        <i class="fas fa-info-circle"></i>
      </button>
    </div>
    
    <AboutModal v-if="showAbout" @close="showAbout = false" />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';
import Website from './Website.vue';
import VisitTimer from './VisitTimer.vue';
import AboutModal from './AboutModal.vue';
import Typed from 'typed.js';

// 状态管理
const isDarkMode = ref(false);
const showAbout = ref(false);
const avatarUrl = ref(import.meta.env.VITE_APP_AVATAR_URL || '/avatar.jpg');
const userName = ref(import.meta.env.VITE_APP_USER_NAME);
let typedInstance = null;

// 计算属性缓存深色模式配置
const darkModeConfig = computed(() => ({
  iconClass: isDarkMode.value ? 'fas fa-sun' : 'fas fa-moon',
  tooltipText: isDarkMode.value ? '切换至浅色模式' : '切换至深色模式',
  hoverColor: isDarkMode.value ? '#ffcc00' : '#666'
}));

// 初始化Typed.js
const initializeTyped = () => {
  const options = {
    strings: ['前端开发者', 'Vue爱好者', '技术探索者'],
    typeSpeed: 100,
    backSpeed: 50,
    loop: true
  };
  typedInstance = new Typed('#typed-output', options);
};

// 生命周期管理
onMounted(() => {
  // 初始化深色模式
  const savedDarkMode = localStorage.getItem('darkMode');
  if (savedDarkMode !== null) {
    isDarkMode.value = savedDarkMode === 'true';
    document.body.classList.toggle('dark-mode', isDarkMode.value);
  }
  
  initializeTyped();
});

onUnmounted(() => {
  if (typedInstance) {
    typedInstance.destroy();
  }
});

// 事件处理
const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;
  document.body.classList.toggle('dark-mode', isDarkMode.value);
  localStorage.setItem('darkMode', isDarkMode.value);
};

const toggleAbout = () => {
  showAbout.value = !showAbout.value;
};
</script>

<style scoped>
.content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  padding: 2rem 1rem;
}

.user-profile-container {
  display: flex;
  align-items: center;
  gap: 2rem;
  width: 100%;
  max-width: 800px;
}

.avatar {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid var(--border-color);
}

.typed-container {
  font-size: 1.2rem;
  margin-top: 0.5rem;
}

.cursor {
  animation: blink 1s step-end infinite;
}

.action-buttons {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

button {
  background: rgba(var(--background-color-rgb), 0.5);
  border: 1px solid var(--border-color);
  border-radius: 50%;
  width: 40px;
  height: 40px;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:hover {
  transform: scale(1.1);
  background: rgba(var(--background-color-rgb), 0.8);
}

@keyframes blink {
  from, to { opacity: 1; }
  50% { opacity: 0; }
}

@media screen and (max-width: 768px) {
  .user-profile-container {
    flex-direction: column;
    text-align: center;
    gap: 1rem;
  }
  
  .avatar {
    width: 100px;
    height: 100px;
  }
}
</style>
