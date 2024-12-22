<template>
  <div style="height: 100vh; margin: 0; padding: 0;">
    <!-- 左侧导航 -->
    <el-aside :width="isCollapsed ? '60px' : '200px'" class="aside-nav">
      <div class="logo-container">
        <img src="/logo.png" alt="logo" class="logo" v-if="!isCollapsed">
        <h4 v-if="!isCollapsed">网址收藏</h4>
        <button @click="toggleCollapse" class="collapse-button">
          <img src="/icons/折叠菜单.svg" class="collapse-icon" />
        </button>
      </div>
      <el-menu
        :default-active="activeIndex"
        class="nav-menu"
        background-color="#333"
        text-color="#fff"
        active-text-color="#409EFF"
      >
        <el-menu-item 
          v-for="(section, index) in sections" 
          :key="index" 
          :index="String(index + 1)"
          @click="scrollToSection(index)"
        >
          <a :href="`#section-${index + 1}`">
            <img :src="section.icon" alt="icon" class="custom-icon" />
            <span v-if="!isCollapsed">{{ section.title }}</span>
          </a>
        </el-menu-item>
      </el-menu>
    </el-aside>

    <!-- 右侧内容区 -->
    <el-container :class="{'collapsed': isCollapsed}" class="main-content" direction="vertical">
      <div 
        class="section" 
        v-for="(section, sIndex) in sections" 
        :key="sIndex"
        :id="`section-${sIndex + 1}`"
      >
        <h2 class="section-title">{{ section.title }}</h2>
        <el-row :gutter="20">
          <el-col 
            :span="isMobile ? 12 : 6" 
            v-for="(item, index) in section.items" 
            :key="index"
          >
            <el-card 
              shadow="hover" 
              class="nav-card"
              @click="openLink(item.url)"
            >
              <div class="nav-card-content">
                <img :src="item.icon" class="nav-icon" />
                <div class="nav-info">
                  <h3>{{ item.title }}</h3>
                  <p v-if="!isMobile">{{ item.description }}</p>
                </div>
              </div>
            </el-card>
          </el-col>
        </el-row>
      </div>
    </el-container>
    <el-footer class="footer">
        © 2024 编程导航 | 所有权利保留
    </el-footer>

    <!-- 滑动到顶部图标 -->
    <div v-show="showScrollTop" class="scroll-top" @click="scrollToTop">
      ↑
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import sectionsData from './sections.json'

const activeIndex = ref('1')
const sections = ref(sectionsData)
const showScrollTop = ref(false)
const isCollapsed = ref(false)
const isMobile = ref(false)

const openLink = (url) => {
  window.open(url, '_blank')
}

const scrollToSection = (index) => {
  const section = document.getElementById(`section-${index + 1}`);
  section?.scrollIntoView({ behavior: 'smooth' });
}

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

const handleScroll = () => {
  showScrollTop.value = window.scrollY > 200;
}

const toggleCollapse = () => {
  isCollapsed.value = !isCollapsed.value;
}

const checkMobile = () => {
  isMobile.value = window.innerWidth <= 768;
  if (isMobile.value) {
    isCollapsed.value = true;
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  window.addEventListener('resize', checkMobile);
  checkMobile();
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  window.removeEventListener('resize', checkMobile);
})
</script>

<style scoped>
.aside-nav {
  background-color: #333;
  color: #fff;
  height: 100%;
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  transition: width 0.3s;
}

.logo-container {
  padding: 20px;
  text-align: center;
  border-bottom: 1px solid #444;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.logo {
  width: 40px;
  height: 40px;
  margin-bottom: 10px;
}

.collapse-button {
  background: none;
  border: none;
  color: #fff;
  cursor: pointer;
  font-size: 16px;
}

.collapse-icon {
  width: 20px;
  height: 20px;
}

.nav-menu {
  border-right: none;
}

.el-menu-item a {
  text-decoration: none;
  color: inherit;
  display: flex;
  align-items: center;
}

.custom-icon {
  width: 20px;
  height: 20px;
  margin-right: 10px;
  vertical-align: middle;
}

.main-content {
  margin-left: 200px;
  background-color: #f5f7fa;
  padding: 20px;
  scroll-behavior: smooth;
  flex: 1;
  min-height: 100vh;
  width: calc(100% - 200px);
  position: relative;
  overflow-y: auto;
  transition: margin-left 0.3s;
}

.main-content.collapsed {
  margin-left: 60px;
  width: calc(100% - 60px);
}

.section {
  margin-bottom: 30px;
  scroll-margin-top: 20px;
  width: 100%;
  padding: 0px;
}

.section-title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 20px;
  padding-left: 10px;
  border-left: 4px solid #409EFF;
}

.nav-card {
  width: 100%;
  cursor: pointer;
  margin-bottom: 0;
  border: none;
  transition: all 0.3s;
}

.nav-card:hover {
  transform: translateY(-5px);
}

.nav-card-content {
  display: flex;
  align-items: center;
  gap: 15px;
}

.nav-icon {
  width: 40px;
  height: 40px;
  border-radius: 8px;
}

.nav-info h3 {
  margin: 0;
  font-size: 16px;
  color: #303133;
}

.nav-info p {
  margin: 5px 0 0;
  font-size: 12px;
  color: #909399;
  line-height: 1.5;
}

.el-menu-item {
  display: flex;
  align-items: center;
  gap: 10px;
}

.footer {
  background-color: #333;
  color: #fff;
  text-align: center;
  padding: 10px 0;
  font-size: 14px;
  width: 100%;
  position: sticky;
  bottom: 0;
}

.scroll-top {
  position: fixed;
  bottom: 80px;
  right: 20px;
  background-color: #409EFF;
  color: white;
  padding: 8px;
  border-radius: 50%;
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
  transition: opacity 0.3s;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
}

:deep(.el-card__body) {
  padding: 15px;
}

.el-row {
  width: 100%;
  margin: 0 !important;
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.el-col {
  display: flex;
  padding: 10px !important;
}

/* Media query for mobile responsiveness */
@media (max-width: 768px) {
  .el-col {
    flex: 0 0 100%;
    max-width: 100%;
  }

  .nav-info p {
    display: none;
  }
}
</style>
