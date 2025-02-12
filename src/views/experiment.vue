<template>

<div class="app-container experiment">
    <div class="experiment-header">
      <h1 class="title">高锰酸钾分解制取氧气</h1>
      <div class="stats">
        <span class="views">
          <el-icon><View /></el-icon>
          924.4k 人观看
        </span>
        <span class="likes">
          <el-icon><Star /></el-icon>
          269 收藏
        </span>
        <span class="hot-tag">HOT</span>
        <div class="info-header">
          <el-button type="primary" >去做实验</el-button>
          <el-button>
            <el-icon><Share /></el-icon>
            分享
          </el-button>
        </div>
      </div>
      
    </div>
    
    <div class="experiment-content">
      
      <div class="video-section">
        <vue3videoPlay v-bind="options" />
      </div>

      <div class="experiment-info">
      
        <el-scrollbar height="550px">
        <div class="info-content">
            
                <div v-for="(item, index) in experimentItems" :key="index" class="info-item">
      <div class="item-header">
        <h3>{{ item.title }}</h3>
        <el-button 
          class="expand-btn" 
          type="text"
          @click="toggleExpand(index)"
        >
          <el-icon>
            <component :is="item.isExpanded ? 'Minus' : 'Plus'" />
          </el-icon>
        </el-button>
      </div>
      <div class="item-content" :class="{ 'expanded': item.isExpanded }">
        <!-- <el-input
          
          v-model="item.content"
          type="textarea"
          :rows="4"
        
          disabled="true"
          @blur="autoSave(index)"
        /> -->

        <span v-if="item.isExpanded">{{ item.content }}</span>
      
      </div>
    </div>
            
  
  </div>
</el-scrollbar>
      </div>
    </div>
  </div>

</template>
  
<script setup>
// 引入视频插件组件和样式
import vue3videoPlay from 'vue3-video-play' // 引入组件
import 'vue3-video-play/dist/style.css' // 引入css
import { reactive } from 'vue';
const options = reactive({
  width: "100%", //播放器宽度 也可以使用px
  height: "100%", //播放器高度  同理可以使用px
  color: "#409eff", //主题色
  title: "", //视频名称
  src: "https://motong-static.oss-cn-beijing.aliyuncs.com/videos/MajorAndJunior/%E6%B0%B4%E7%9A%84%E6%B2%B8%E8%85%BE.mp4", //视频源
  muted: false, //静音
  webFullScreen: false,
  speedRate: ["0.5", "0.75", "1.0", "1.25", "1.5", "2.0"], //播放倍速
  autoPlay: false, //自动播放
  loop: false, //循环播放
  mirror: false, //镜像画面
  ligthOff: false, //关灯模式
  volume: 0.3, //默认音量大小
  control: true, //是否显示控制
  controlBtns: [
    "audioTrack",
    "quality",
    "speedRate",
    "volume",
    "setting",
    "pip",
    "pageFullScreen",
    "fullScreen",
  ], //显示所有按钮,
})

  const experimentItems = ref([
  {
    title: '实验目的：',
    content: '学习实验室制取氧气的方法之一——加热高锰酸钾制氧气。',
    placeholder: '请填写实验目的',
    isExpanded: false
  },
  {
    title: '实验原理：',
    content: '',
    placeholder: '请填写实验原理',
    isExpanded: false
  },

  {
    title: '实验目标：',
    content: '实验目标：',
    placeholder: '请填写实验原理',
    isExpanded: false
  },
  {
    title: '实验用品：',
    content: '实验用品：',
    placeholder: '请填写实验用品',
    isExpanded: false
  },
  {
    title: '实验步骤：',
    content: '实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：实验步骤：',
    placeholder: '请填写实验步骤',
    isExpanded: false
  }
])

const toggleExpand = (index) => {
  experimentItems.value[index].isExpanded = !experimentItems.value[index].isExpanded
}

const autoSave = (index) => {
  // 这里添加自动保存逻辑
  console.log('自动保存:', experimentItems.value[index].content)
}
</script>
  
<style lang='scss' scoped>

.experiment {
  padding: 24px;
  margin-top: 70px;

  .experiment-header {
    margin-bottom: 24px;

    .title {
      font-size: 24px;
      color: #303133;
      margin-bottom: 16px;
    }

    .stats {
      display: flex;
      align-items: center;
      gap: 24px;
      color: #909399;
      font-size: 14px;

      .views, .likes {
        display: flex;
        align-items: center;
        gap: 4px;
      }

      .hot-tag {
        background: #ff9900;
        color: white;
        padding: 2px 8px;
        border-radius: 4px;
        font-size: 12px;
      }

      .info-header {
        position: absolute;
        right: 0;
        padding: 24px;
      }
    }
  }

  .experiment-content {
    display: flex;
    gap: 24px;

    .video-section {
      flex: 1;
      
      .video-player {
        aspect-ratio: 16/9;
        background: #000;
        border-radius: 8px;
        overflow: hidden;

        .video-placeholder {
          width: 100%;
          height: 100%;
          display: flex;
          align-items: center;
          justify-content: center;
          color: #fff;
          font-size: 18px;
          position: relative;

          .play-icon {
            font-size: 48px;
            margin-left: 12px;
          }
        }
      }
    }

    .experiment-info {
      width: 40%;
      background: #fff;
      border-radius: 20px;
      padding: 20px;
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);

    

      .info-content {
        
  .info-item {
    margin-bottom: 20px;
    border-bottom: 1px solid #ebeef5;
    padding-bottom: 16px;

    &:last-child {
      border-bottom: none;
    }

    .item-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;

      h3 {
        font-size: 16px;
        color: #303133;
        margin: 0;
        min-width: 100px;
      }

      .expand-btn {
        padding: 4px;
        color: #909399;
        margin-right: 15px;
        &:hover {
          color: #409EFF;
         
        }
      }
    }

    .item-content {
      position: relative;
      margin-right: 20px;
      .preview-content {
        padding: 8px 12px;
        color: #606266;
        background: #f5f7fa;
        border-radius: 4px;
        cursor: pointer;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        min-height: 36px;
        line-height: 20px;
        
        &:hover {
          background: #ecf5ff;
        }
      }

      &.expanded {
        :deep(.el-textarea__inner) {
          border-color: #409EFF;
          background: #fff;
          
          &:focus {
            box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.1);
          }
        }
      }
    }
  }
}
    }
  }
}
</style>
  