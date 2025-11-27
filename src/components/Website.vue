<template>
  <div class="website-container">
    <div class="search-box">
      <input 
        v-model="searchQuery" 
        type="text" 
        placeholder="搜索网站..." 
        @input="handleSearch"
      >
    </div>
    <div class="site-grid">
      <div 
        v-for="site in filteredSites" 
        :key="site.id"
        class="site-box"
        @click="handleSiteClick(site)"
        @mouseenter="handleMouseEnter(site.id)"
        @mouseleave="handleMouseLeave"
      >
        <div class="site-icon">
          <i :class="hoveredSiteId === site.id ? site.hoverIcon : site.icon"></i>
        </div>
        <div class="site-name">{{ site.name }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { debounce } from 'perfect-debounce';

// 网站数据
const sites = ref([
  { id: 1, name: 'GitHub', url: 'https://github.com', icon: 'fab fa-github', hoverIcon: 'fab fa-github-alt' },
  { id: 2, name: '掘金', url: 'https://juejin.cn', icon: 'fas fa-fire', hoverIcon: 'fas fa-fire-alt' },
  { id: 3, name: 'Stack Overflow', url: 'https://stackoverflow.com', icon: 'fab fa-stack-overflow', hoverIcon: 'fab fa-stack-overflow' },
  { id: 4, name: 'MDN', url: 'https://developer.mozilla.org', icon: 'fas fa-book', hoverIcon: 'fas fa-book-open' },
  // 可添加更多网站
]);

// 状态管理
const searchQuery = ref('');
const hoveredSiteId = ref(null);
const filteredSites = ref([...sites.value]);

// 搜索处理（防抖优化）
const handleSearch = debounce((e) => {
  const query = e.target.value.toLowerCase().trim();
  filteredSites.value = query 
    ? sites.value.filter(site => site.name.toLowerCase().includes(query))
    : [...sites.value];
}, 300);

// 事件处理
const handleSiteClick = (site) => {
  if (site.url) {
    window.open(site.url, '_blank', 'noopener,noreferrer');
  }
};

const handleMouseEnter = (id) => {
  hoveredSiteId.value = id;
};

const handleMouseLeave = () => {
  hoveredSiteId.value = null;
};

// 初始化
onMounted(() => {
  // 可以从本地存储加载自定义网站
  const savedSites = localStorage.getItem('customSites');
  if (savedSites) {
    try {
      const parsed = JSON.parse(savedSites);
      sites.value = [...sites.value, ...parsed];
      filteredSites.value = [...sites.value];
    } catch (e) {
      console.error('Failed to parse custom sites', e);
    }
  }
});
</script>

<style scoped>
.website-container {
  width: 100%;
  max-width: 1000px;
  padding: 1rem;
}

.search-box {
  margin-bottom: 1.5rem;
}

.search-box input {
  width: 100%;
  padding: 0.8rem 1rem;
  border: 1px solid var(--border-color);
  border-radius: var(--border-radius);
  background-color: rgba(var(--background-color-rgb), 0.5);
  font-size: 1rem;
  outline: none;
  transition: all 0.3s ease;
}

.search-box input:focus {
  border-color: var(--link-hover-color);
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.2);
}

.site-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  gap: 1rem;
}

.site-box {
  padding: 1.5rem 0.5rem;
  backdrop-filter: blur(10px);
  border-radius: var(--border-radius);
  border: 1px solid var(--border-color);
  background-color: rgba(var(--background-color-rgb), 0.2);
  cursor: pointer;
  transition: all 0.3s ease;
  text-align: center;
}

.site-box:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  border-color: var(--link-hover-color);
}

.site-icon {
  font-size: 1.8rem;
  margin-bottom: 0.5rem;
  transition: all 0.3s ease;
}

.site-name {
  font-size: 0.9rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
