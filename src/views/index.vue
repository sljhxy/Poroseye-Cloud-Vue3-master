<template>
  <div class="app-container home">
    <!-- 添加顶部导航栏 -->
    <div class="top-navbar">
      <div class="nav-left">
        <h1>POROSEYE实验·精品实验</h1>
        <span class="exp-count">52个</span>
      </div>
      <div class="nav-right">
        <el-input
          v-model="searchText"
          placeholder="搜索实验"
          prefix-icon="Search"
          class="search-input"
        />
        <!-- <el-button type="primary" class="create-btn">
          <el-icon><Plus /></el-icon>新建实验
        </el-button> -->
      </div>
    </div>
    
    <!-- 左侧导航栏 -->
    <div class="content-wrapper">

      <div class="sidebar">
      <div class="sidebar-header">
        <span 
          :class="{ 'active-tab': activeTab === 'chapter' }"
          @click="switchTab('chapter')"
        >章节</span>
        <span 
          :class="{ 'active-tab': activeTab === 'knowledge' }"
          @click="switchTab('knowledge')"
        >知识点</span>
      </div>
      <div class="chapter-list">
        <div class="chapter-item main-chapter">
          <el-button size="large" class="btn-chapter"
          @click="showAboutDialog = true"
          >人教版·八年级上 <el-icon><ArrowRight /></el-icon></el-button>
        </div>
        <div 
          v-for="(chapter, index) in chapterList" 
          :key="index"
          class="chapter-item"
          :class="{ active: currentChapter === chapter.id }"
          @click="clickDialog(chapter)"
        >
          <span>{{ chapter.title }}</span>
          <el-icon><ArrowRight /></el-icon>
        </div>
      </div>
    </div>



    <!-- 右侧内容区域 -->
    <div class="main-content">
      <div class="exp-list-section">
        <el-row :gutter="24">
          <el-col :span="6" v-for="item in expList" :key="item.id">
            <div class="exp-item">

              <image-preview :src="item.img" :alt="item.title" />
              <div class="exp-preview">
                
                <div class="favorite-btn" @click.stop="toggleFavorite(item)">
                  <el-icon :class="{ 'is-favorite': item.isFavorite }">
                    <Star />
                  </el-icon>
                </div>
              </div>
              <div class="exp-info">
                <h3>{{ item.title }}</h3>
                <div class="exp-stats">
                  <span><el-icon style="position: relative;top: 2px;"><View /></el-icon> {{ item.views }}次查看</span>
                  <el-dropdown trigger="click">
                    <el-button size="small" circle>
                      <el-icon><More /></el-icon>
                    </el-button>
                    <template #dropdown>
                      <el-dropdown-menu>
                        <el-dropdown-item @click="watchVideo(item)">
                          <el-icon><VideoPlay /></el-icon>在线视频
                        </el-dropdown-item>
                        <el-dropdown-item @click="startExperiment(item)">
                          <el-icon><VideoPlay /></el-icon>开始实验
                        </el-dropdown-item>
                      </el-dropdown-menu>
                    </template>
                  </el-dropdown>
                </div>
              </div>
            </div>
          </el-col>
        </el-row>

      </div>
    </div>
    </div>


        <!-- 教材版本弹框 -->
      <el-dialog
        v-model="showAboutDialog"
        width="800px"
        title="选择教材"
        style="border-radius: 15px;position: relative;top: 50px;text-align: center;"
        :close-on-click-modal="false"
        class="textbook-dialog"
        destroy-on-close>
        <div class="textbook-content">
    <div class="education-level">

      <div 
        v-for="(level, index) in educationLevels" 
        :key="index"
        class="level-item"
        :class="{ active: currentLevel === level.value }"
        @click="handleLevelChange(level.value)"
      >
        {{ level.label }}
      </div>
    </div>
    <el-divider />
    <div class="version-tabs">
      <div 
    v-for="(version, index) in versionList" 
    :key="index"
    class="version-item"
    :class="{ active: currentVersion === version.value }"
    @click="handleVersionChange(version.value)"
  >
    {{ version.label }}
  </div>
    </div>
    <div class="textbook-list">
      <div class="textbook-row">
        <div class="textbook-item all-books">
          <div class="cover-wrapper">
            <img src="/src/assets/images/all.jpg" alt="全部教材" />
          </div>
          <div class="overlay">
              <span>所有教材</span><br>
              <span>全部资源</span>
            </div>
        </div>
        
        <div class="textbook-item" v-for="item in textbookList" :key="item.id">
          <div class="cover-wrapper">
            <img :src="item.cover" :alt="item.title" class="textbook-cover"/>
          </div>
          <div class="book-info">
              <span class="version">{{ item.versionName }}</span>
              <span class="grade">{{ item.grade }}</span>
            </div>
        </div>
      </div>
    </div>
  </div>
  <template #footer>
    <el-divider />
    <div class="dialog-footer">
      <div class="dialog-footer-btn">
        <el-button  @click="cancleDialog">取消</el-button>
        <el-button  type="primary" @click="confirmSelect">确定</el-button>
      </div>
    
    </div>
  </template>
    </el-dialog>
  </div>
</template>

<script setup name="Index">

import { ref } from 'vue'

import {useRoute, useRouter} from 'vue-router'
// 添加关于弹框控制变量
const showAboutDialog = ref(false)
const searchText = ref('')

const isFocused = ref(false); // 响应式状态，记录焦点状态
const router = useRouter()

// 添加教材数据
const textbookList = ref([
  {
    id: 1,
    title: '化学',
    grade: '九年级上',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版人教版人教版人教版人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    versionName:'人教版',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  {
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },{
    id: 2,
    title: '化学',
    grade: '九年级下',
    cover: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png'
  },
  // ... 更多教材数据
])
// 推荐实验列表
const recommendList = ref([
  {
    id: 1,
    title: '高考真题模拟',
    tag: '专题',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 2500
  },
  {
    id: 2,
    title: '学生必做实验',
    tag: '专题',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 3600
  },
  {
    id: 3,
    title: '打点计时器',
    tag: '知识专题',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 1800
  },
  {
    id: 4,
    title: '力的合成实验',
    tag: '知识专题',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 2100
  },
  {
    id: 5,
    title: '三种帮电方式',
    tag: '知识专题',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 1500
  }
])



//弹框确认按钮
const confirmSelect = () => {
  // 处理确认选择逻辑
  showAboutDialog.value = false
}

//弹框取消按钮
const cancleDialog = () => {
  showAboutDialog.value = false
  currentLevel.value = 'primary' // 默认选中小学

}
// 筛选标签
const filterTags = ref([
  { label: '全部', value: 'all', active: true },
  { label: '探究实验', value: 'research', active: false },
  { label: '学案', value: 'case', active: false },
  { label: '互动课件', value: 'interactive', active: false }
])

// 修改实验列表数据，添加 isFavorite 属性
const expList = ref([
  {
    id: 1,
    title: '2023江苏卷第10题',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 2500,
    isFavorite: false
  },
  {
    id: 2,
    title: '探究两个互成角度的力的合成规律的实验',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 6000,
    isFavorite: true
  },
  {
    id: 3,
    title: '探究小车速度随时间变化的规律',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 3900,
    isFavorite: false
  },
  {
    id: 4,
    title: '探究小车速度随时间变化的规律',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 3900,
    isFavorite: false
  },
  {
    id: 5,
    title: '探究小车速度随时间变化的规律',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 3900,
    isFavorite: false
  },
  {
    id: 6,
    title: '探究小车速度随时间变化的规律',
    img: 'https://motong-java.oss-accelerate.aliyuncs.com/motong/2025/1/3/8b85d5727b916bca0e356aa61f0d837b_400-340DummyCover.png',
    views: 3900,
    isFavorite: false
  },
  {
    id: 1,
    title: '化学',
    version: '新人教版',
    grade: '九年级上',
    cover: '...'
  },
  {
    id: 2,
    title: '化学',
    version: '人教版',
    grade: '九年级下',
    cover: '...'
  },

])
const toggleTag = (tag) => {
  filterTags.value.forEach(t => t.active = false)
  tag.active = true
}
// 添加处理函数
const watchVideo = (item) => {
  console.log('观看视频:', item.title)

  router.push('/experiment')
  
  // 这里添加观看视频的逻辑
}

const startExperiment = (item) => {
  console.log('开始实验:', item.title)
  location.href = 'https://motong-pc.oss.poroseye.com/?d=%E6%8E%A2%E7%A9%B6%E5%87%B8%E9%80%8F%E9%95%9C%E6%88%90%E5%83%8F%E7%9A%84%E8%A7%84%E5%BE%8B&title=%E6%8E%A2%E7%A9%B6%E5%87%B8%E9%80%8F%E9%95%9C%E6%88%90%E5%AE%9E%E5%83%8F%E7%9A%84%E8%A7%84%E5%BE%8B'
  // 这里添加开始实验的逻辑
}
// 添加收藏切换方法
const toggleFavorite = (item) => {
  item.isFavorite = !item.isFavorite;
  // 这里可以添加收藏相关的后端接口调用
}
// 添加章节数据
const chapterList = ref([
  { id: 1, title: '第一章 机械运动' },
  { id: 2, title: '第二章 声现象' },
  { id: 3, title: '第三章 物态变化' },
  { id: 4, title: '第四章 光现象' },
  { id: 5, title: '第五章 透镜及其应用' },
  { id: 6, title: '第六章 质量与密度' }
])

const currentChapter = ref(1)

const selectChapter = (chapter) => {
  currentChapter.value = chapter.id
  // 这里可以根据选中的章节更新右侧的实验列表
  // 模拟更新实验列表
  expList.value = expList.value.map(item => ({
    ...item,
    title: `${chapter.title}-${item.id}号实验`
  }))
}
// 添加知识点数据
const knowledgeList = ref([
  { id: 1, title: '力学基础知识' },
  { id: 2, title: '运动与力' },
  { id: 3, title: '压强与浮力' },
  { id: 4, title: '功与机械能' },
  { id: 5, title: '光学现象' },
  { id: 6, title: '声学知识' },
  { id: 7, title: '电学基础' },
  { id: 8, title: '磁现象' }
])

// 添加切换相关的变量
const activeTab = ref('chapter')
const currentSelected = ref(1)
const currentList = computed(() => {
  return activeTab.value === 'chapter' ? chapterList.value : knowledgeList.value
})

// 切换标签方法
const switchTab = (tab) => {
  activeTab.value = tab
  currentSelected.value = 1
  // 更新实验列表
  updateExpList()
}

// 选择项目方法
const selectItem = (item) => {
  currentSelected.value = item.id
  // 更新实验列表
  updateExpList()
}

// 更新实验列表方法
const updateExpList = () => {
  const prefix = activeTab.value === 'chapter' ? '章节' : '知识点'
  const selectedItem = currentList.value.find(item => item.id === currentSelected.value)
  
  expList.value = expList.value.map(item => ({
    ...item,
    title: `${prefix}-${selectedItem?.title}-${item.id}号实验`
  }))
}
// 添加学段相关数据
const educationLevels = ref([
  { label: '小学', value: 'primary' },
  { label: '初中', value: 'junior' },
  { label: '高中', value: 'senior' }
])

// 当前选中的学段
const currentLevel = ref('primary') // 默认选中小学

// 处理学段切换
const handleLevelChange = (level) => {
  currentLevel.value = level
  // 可以在这里添加其他联动逻辑，比如更新教材列表
  updateTextbookList(level)
}



// 添加版本相关数据
const versionList = ref([
  { label: '全部', value: 'all' },
  { label: '新人教版', value: 'new_pep' },
  { label: '人教版', value: 'pep' },
  { label: '人教版', value: 'pep1' },
  { label: '人教版', value: 'pep2' },
  { label: '人教版', value: 'pep3' },
  { label: '人教版', value: 'pep4' },
  { label: '人教版', value: 'pep5' },
  { label: '人教版', value: 'pep51' },
  { label: '人教版', value: 'pep52' },
  { label: '人教版', value: 'pep53' },
  { label: '科粤版', value: 'guangdong' }
])

// 当前选中的版本
const currentVersion = ref('all') // 默认选中全部

// 处理版本切换
const handleVersionChange = (version) => {
  currentVersion.value = version
  // 这里可以添加版本切换的相关逻辑
  updateTextbookList(currentLevel.value, version)
}

// 更新教材列表方法也需要更新，添加版本参数
const updateTextbookList = (level, version) => {
  console.log('更新教材列表:', level, version)
  // 这里添加根据学段和版本筛选教材的逻辑
}
// 移除原有的 selectChapter 方法，改用 selectItem
</script>

<style scoped lang="scss">
.home {
  display: flex;
  flex-direction: column;
  padding: 24px;
  margin-top: 70px;
  .top-navbar {
    background: #fff;
    padding: 16px 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.05);
    margin-bottom: 24px;

    .nav-left {
      display: flex;
      align-items: center;
      gap: 12px;

      h1 {
        font-size: 20px;
        color: #303133;
        margin: 0;
      }

      .exp-count {
        color: #909399;
        font-size: 14px;
      }
    }

    .nav-right {
      display: flex;
      align-items: center;
      gap: 16px;

      .search-input {
        width: 300px;
      }

      .create-btn {
        display: flex;
        align-items: center;
        gap: 4px;
      }
    }
  }
  .content-wrapper {
    display: flex;
    padding: 0 24px 24px;
    .sidebar {
    
    width: 240px;
    background: #fff;
    border-right: 1px solid #e6e6e6;
    height: 100vh;
    .sidebar-header {
      display: flex;
      border-bottom: 1px solid #e6e6e6;
      span {
        flex: 1;
        text-align: center;
        padding: 15px 0;
        cursor: pointer;
        color: #606266;
        &.active-tab {
          color: #ff9900;
          border-bottom: 2px solid #ff9900;
          font-weight: bold;
          &:hover {
            color: #ff9900;
            font-weight: bold;
          }
        }
      }
    }
    .chapter-list {
      padding: 10px 0;
      .chapter-item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 12px 20px;
        cursor: pointer;
        color: #606266;
        &:hover {
          background: #f5f7fa;
        }
        &.active {
          color:#ff9900;
          background: #ecf5ff;
          font-weight: bold;
        }
        &.main-chapter {
          font-weight: 500;
          color: #303133;


          //教材版本按钮样式
          .btn-chapter{
            border-radius: 10px;
            width: 200px;
            &:hover {
              color: #606266;
            }
          }

          // .focused-style {
          //   background-color: #4CAF50;
          //   color: white;
          //   border-color: #45a049;
          //   box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
          // }
        }
        .el-icon {
          font-size: 16px;
        }
      }
    }
  }
  .main-content {
    flex: 1;
    padding: 24px;
    background: #f5f7fa;
    min-height: 100vh;
    .header {
      background: #fff;
      padding: 16px 24px;
      border-radius: 8px;
      margin-bottom: 24px;
      box-shadow: 0 1px 4px rgba(0,0,0,0.05);
      .header-left {
        h2 {
          font-size: 20px;
          margin-bottom: 16px;
          color: #303133;
        }
        .filter-tags {
          display: flex;
          gap: 8px;
          .el-tag {
            cursor: pointer;
            transition: all 0.3s;
            &.active {
              background-color: #409EFF;
              color: white;
            }
            &:hover {
              transform: translateY(-1px);
            }
          }
        }
      }
      .header-right {
        display: flex;
        gap: 12px;
        margin-top: 16px;
        .search-input {
          width: 300px;
        }
      }
    }
    .exp-list-section {
      .exp-item {
        background: #fff;
        border-radius: 8px;
        overflow: hidden;
        transition: all 0.3s;
        margin-bottom: 24px;
        &:hover {
          transform: translateY(-2px);
          box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        .exp-info {
          padding: 0 10px 10px;
          h3 {
            font-size: 15px;
            margin-bottom: 12px;
            color: #303133;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
            line-height: 1.4;
          }
          .exp-stats {
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: #999;
            font-size: 14px;
            padding: 0 4px;
            span {
              flex: 1;
              margin-right: 12px;
            }
            .el-dropdown {
              margin-left: auto;
            }
          }
        }
      }
    }
  }
}
.el-dropdown-menu {
  .el-dropdown-item {
    display: flex;
    align-items: center;
    gap: 5px;
  }
}
.exp-preview {
  .favorite-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 32px;
    height: 32px;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s;

    &:hover {
      transform: scale(1.1);
    }

    .el-icon {
      font-size: 20px;
      color: #909399;

      transition: all 0.3s;

      &.is-favorite {
        color: #f56c6c;

      }
    }
  }
  }
 
}
.el-dropdown-menu {
  .el-dropdown-item {
    display: flex;
    align-items: center;
    gap: 5px;
  }
}
.exp-preview {
  .favorite-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 32px;
    height: 32px;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s;

    &:hover {
      transform: scale(1.1);
    }

    .el-icon {
      font-size: 20px;
      color: #909399;

      transition: all 0.3s;

      &.is-favorite {
        color: #f56c6c;

      }
    }
  }
  }
 


//弹框样式
.textbook-dialog {

  max-height: 100px;
    overflow-y: auto;
    // padding: 20px;
    &::-webkit-scrollbar {
      width: 6px;
    }
    
    &::-webkit-scrollbar-thumb {
      background: #c0c4cc;
      border-radius: 3px;
    }
    
    &::-webkit-scrollbar-track {
      background: #f5f7fa;
    }
  :deep(.el-dialog__header) {
    margin: 0;
    padding: 20px 24px;
    border-bottom: 1px solid #e6e6e6;

    text-align: center;
    
    .el-dialog__title {
      font-size: 18px;
      font-weight: bold;

    }
  }

  .textbook-content {



    .education-level {
      position: sticky;
      top: 0;
      background: white;
      z-index: 2;
      padding: 10px 0;
      display: flex;
      justify-content: center;
      gap: 16px;
      // margin: 20px 0;
      padding: 4px;
      background: #f5f7fa;
      border-radius: 30px;
      width: fit-content;
      margin: 0px auto;
      
      .level-item {
        padding: 10px 40px;
        border-radius: 24px;
        cursor: pointer;
        color: #606266;
        background: #f5f7fa;
        font-size: 16px;
        transition: all 0.3s ease;

        &:hover:not(.active) {
          color: #409EFF;
        }

        &.active {
          background: white;
          color: #409EFF;
          font-weight: bold;
          box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }
      }
    }
    .version-tabs {
      display: flex;
      flex-wrap: wrap; // 添加换行
      justify-content: flex-start; // 改为左对齐
      gap: 5px;
      margin: 20px 0;
      padding: 4px;
      // background: #f5f7fa;
      border-radius: 30px;
      margin: 20px auto;
      width: 100%; // 设置宽度为100%
      .version-item {
        padding: 8px 24px;
        border-radius: 10px;
        cursor: pointer;
        // color: #606266;
        background: #f5f7fa;
        font-size: 14px;
        transition: all 0.3s ease;
        white-space: nowrap; // 防止文字换行
        margin: 4px; // 添加外边距

        &.active {
          background: #e8f6ff;
          color: #409EFF;
          font-weight: bold;
          box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }   
    
      }
    }

    .version-buttons {
      display: flex;
      justify-content: center;
      gap: 12px;
      margin-bottom: 24px;

      .version-btn {
        padding: 8px 24px;
        background: #f5f7fa;
        color: #606266;
        border-radius: 4px;
        cursor: pointer;
        transition: all 0.3s;
        font-size: 14px;

        &:hover:not(.active) {
          background: #e4e7ed;
        }

        &.active {
          background: #409EFF;
          color: white;
        }
      }
    
    }

    .textbook-list {
      padding: 0px;

      .textbook-row {
        display: grid;
        grid-template-columns: repeat(6, 1fr);
        gap: 20px;
      }

      .textbook-item {
        .cover-wrapper {
          position: relative;
          aspect-ratio: 3/4;
          border-radius: 8px;
          overflow: hidden;
          box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);

          cursor: pointer;
          transition: all 0.3s;

          &:hover {
            transform: translateY(-5px);
          }

          img {
            width: 100%;
            height: 100%;
            object-fit: cover;
          }
        }
      }
    }

  }
}


</style>

















