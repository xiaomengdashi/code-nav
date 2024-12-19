<template>
  <el-container style="height: 100vh; margin: 0; padding: 0;">
    <!-- 左侧导航 -->
    <el-aside width="200px" style="background-color: #333; color: #fff;">
      <div class="logo-container">
        <img src="/logo.png" alt="logo" class="logo">
        <h2>网址收藏</h2>
      </div>
      <el-menu
        :default-active="activeIndex"
        class="nav-menu"
        background-color="#333"
        text-color="#fff"
        active-text-color="#409EFF"
      >
        <el-menu-item v-for="(section, index) in sections" :key="index" :index="index + 1">
          <a :href="`#section-${index + 1}`">
            <img :src="section.icon" alt="icon" class="custom-icon" />
            <span>{{ section.title }}</span>
          </a>
        </el-menu-item>
      </el-menu>
    </el-aside>

    <!-- 右侧内容区 -->
    <el-container direction="vertical">
      <el-main class="main-content">
        <div 
          class="section" 
          v-for="(section, sIndex) in sections" 
          :key="sIndex"
          :id="`section-${sIndex + 1}`"
        >
          <h2 class="section-title">{{ section.title }}</h2>
          <el-row :gutter="20">
            <el-col 
              :span="6" 
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
                    <p>{{ item.description }}</p>
                  </div>
                </div>
              </el-card>
            </el-col>
          </el-row>
        </div>
      </el-main>
      <el-footer class="footer">
        © 2024 编程导航 | 所有权利保留
      </el-footer>
    </el-container>
  </el-container>
</template>

<script setup>
import { ref } from 'vue'
import sectionsData from './sections.json'

const activeIndex = ref('1')
const sections = ref(sectionsData)

const openLink = (url) => {
  window.open(url, '_blank')
}
</script>

<style scoped>
.logo-container {
  padding: 20px;
  text-align: center;
  border-bottom: 1px solid #444;
}

.logo {
  width: 40px;
  height: 40px;
  margin-bottom: 10px;
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
  background-color: #f5f7fa;
  padding: 20px;
  overflow-y: auto;
  scroll-behavior: smooth;
  flex: 1;
}

.section {
  margin-bottom: 30px;
  scroll-margin-top: 20px;
}

.section-title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 20px;
  padding-left: 10px;
  border-left: 4px solid #409EFF;
}

.nav-card {
  cursor: pointer;
  margin-bottom: 20px;
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
}

:deep(.el-card__body) {
  padding: 15px;
}
</style>
