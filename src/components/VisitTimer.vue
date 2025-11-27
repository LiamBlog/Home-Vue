<template>
  <div class="timer-container">
    <div class="date-info">
      <span>{{ dateTime.dateOnly }}</span>
      <span>{{ dateTime.weekday }}</span>
    </div>
    <div class="time-display">
      {{ dateTime.timeWithoutSeconds }}
    </div>
    <div class="visit-duration" @click="toggleCalendar">
      已访问: {{ timeUnits.hours }}:{{ timeUnits.minutes }}:{{ timeUnits.seconds }}
    </div>
    <Calendar v-if="showCalendar" :pinned="isCalendarPinned" @toggle-pin="isCalendarPinned = !isCalendarPinned" />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';
import Calendar from './Calendar.vue';

// 时间管理逻辑封装
const useTimeManager = () => {
  const startTime = ref(Date.now());
  const currentTime = ref(Date.now());
  let timer = null;

  onMounted(() => {
    const updateTime = () => {
      currentTime.value = Date.now();
      timer = requestAnimationFrame(updateTime);
    };
    timer = requestAnimationFrame(updateTime);
  });

  onUnmounted(() => {
    if (timer) cancelAnimationFrame(timer);
  });

  // 计算时间单位
  const timeUnits = computed(() => {
    const totalSeconds = Math.floor((currentTime.value - startTime.value) / 1000);
    return {
      hours: Math.floor(totalSeconds / 3600).toString().padStart(2, '0'),
      minutes: Math.floor((totalSeconds % 3600) / 60).toString().padStart(2, '0'),
      seconds: (totalSeconds % 60).toString().padStart(2, '0')
    };
  });

  // 格式化日期时间
  const dateTime = computed(() => {
    const now = new Date(currentTime.value);
    return {
      dateOnly: now.toLocaleDateString('zh-CN', { year: 'numeric', month: 'long', day: 'numeric' }),
      weekday: now.toLocaleDateString('zh-CN', { weekday: 'long' }),
      timeWithoutSeconds: now.toLocaleTimeString('zh-CN', { hour: '2-digit', minute: '2-digit' })
    };
  });

  return { timeUnits, dateTime };
};

// 组件状态
const showCalendar = ref(false);
const isCalendarPinned = ref(false);
const { timeUnits, dateTime } = useTimeManager();

// 日历切换
const toggleCalendar = () => {
  if (!isCalendarPinned.value) {
    showCalendar.value = !showCalendar.value;
  }
};
</script>

<style scoped>
.timer-container {
  text-align: center;
  padding: 1rem;
  border-radius: var(--border-radius);
  backdrop-filter: blur(10px);
  border: 1px solid var(--border-color);
  background-color: rgba(var(--background-color-rgb), 0.2);
  width: 100%;
  max-width: 300px;
}

.date-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
}

.time-display {
  font-size: 1.8rem;
  font-weight: bold;
  margin: 0.5rem 0;
}

.visit-duration {
  font-size: 0.9rem;
  color: var(--secondary-text-color);
  cursor: pointer;
  margin-top: 0.5rem;
  transition: color 0.3s ease;
}

.visit-duration:hover {
  color: var(--link-hover-color);
}
</style>
